---
title : "Validation & Analysis"
date : 2026-07-10 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---


### Test luồng upload end-to-end

```bash
# 1. Chạy dev server
cd candidate-app
npm run dev
# → http://localhost:5173

# 2. Mở browser, chọn job và upload file PDF test
# 3. Theo dõi Lambda GetPresignedUrl logs
aws logs tail /aws/lambda/hireflow-get-presigned-url --follow --format short

# 4. Kiểm tra file đã vào Quarantine
aws s3 ls s3://hireflow-quarantine-226593872610/web/ --recursive

# 5. Theo dõi Lambda FileValidator logs
aws logs tail /aws/lambda/hireflow-file-validator --follow --format short

# 6. Kiểm tra file đã copy sang Clean bucket
aws s3 ls s3://hireflow-clean-226593872610/web/ --recursive

# 7. File Quarantine đã bị xóa sau PASS
aws s3 ls s3://hireflow-quarantine-226593872610/web/
# (không còn file)
```

**Expected output trong FileValidator logs sau khi upload file hợp lệ:**

```
[INFO] Validating: s3://hireflow-quarantine-226593872610/web/job-001/uuid.pdf
[INFO] PASS — đã copy sang Clean bucket: web/job-001/uuid.pdf
[INFO] Deleted from Quarantine: web/job-001/uuid.pdf
```

### Test các trường hợp validation fail

```bash
# Test 1: Upload file có extension sai (đổi tên .exe thành .pdf)
# → Lambda GetPresignedUrl từ chối ngay tại bước lấy URL
# Expected: HTTP 400 {"error": "Định dạng không hợp lệ. Chỉ nhận: PDF, DOCX, DOC"}

# Test 2: Upload file vượt 5MB
# → Frontend báo lỗi trước khi gọi API
# Expected: message "File quá lớn. Tối đa 5MB."

# Test 3: Upload file PDF giả (đổi tên file .txt thành .pdf)
aws s3 cp fake.txt s3://hireflow-quarantine-226593872610/web/job-001/fake.pdf \
  --content-type application/pdf
# → FileValidator đọc magic bytes → không thấy '%PDF' → FAIL
# → SNS alert gửi đến HR email
# Expected logs: [WARN] Validation FAIL — MIME không hợp lệ

# Test 4: Upload cùng file 2 lần (SHA-256 trùng)
# Lưu ý: SHA-256 chỉ được ghi vào DynamoDB sau khi Lambda Extract chạy xong
# (bước tiếp theo trong pipeline — xem Workshop 1).
# Nếu Extract chưa chạy, lần upload thứ 2 sẽ PASS FileValidator bình thường.
# Khi Extract đã chạy và ghi hash vào DB, upload lần 3+ mới bị chặn.
# → FileValidator query DynamoDB GSI sha256_hash-index → tìm thấy hash → FAIL
# Expected logs: [WARN] Validation FAIL — File trùng lặp (SHA-256: ...)
```

### Kiểm tra presigned URL hết hạn

```bash
# Lấy presigned URL
curl "https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod/apply/presigned-url?job_id=job-001&file_name=cv.pdf"

# Đợi 15 phút, sau đó thử upload
curl -X PUT "<presigned_url>" \
  -H "Content-Type: application/pdf" \
  --data-binary @cv.pdf
# Expected: HTTP 403 AccessDenied — URL đã hết hạn
```



