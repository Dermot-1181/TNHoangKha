---
title : "Validation & Analysis"
date : 2026-07-10
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---



### End-to-end upload test

```bash
# 1. Start dev server
cd candidate-app
npm run dev
# → http://localhost:5173

# 2. Open browser, select a job and upload a test PDF file
# 3. Watch Lambda GetPresignedUrl logs
aws logs tail /aws/lambda/hireflow-get-presigned-url --follow --format short

# 4. Verify file arrived in Quarantine
aws s3 ls s3://hireflow-quarantine-226593872610/web/ --recursive

# 5. Watch Lambda FileValidator logs
aws logs tail /aws/lambda/hireflow-file-validator --follow --format short

# 6. Verify file copied to Clean bucket
aws s3 ls s3://hireflow-clean-226593872610/web/ --recursive

# 7. Confirm Quarantine file was deleted after PASS
aws s3 ls s3://hireflow-quarantine-226593872610/web/
# (no files remaining)
```

**Expected FileValidator logs after a valid upload:**

```
[INFO] Validating: s3://hireflow-quarantine-226593872610/web/job-001/uuid.pdf
[INFO] PASS — copied to Clean bucket: web/job-001/uuid.pdf
[INFO] Deleted from Quarantine: web/job-001/uuid.pdf
```

### Validation failure test cases

```bash
# Test 1: Upload file with wrong extension (.exe renamed to .pdf)
# → Lambda GetPresignedUrl rejects at the URL generation step
# Expected: HTTP 400 {"error": "Định dạng không hợp lệ. Chỉ nhận: PDF, DOCX, DOC"}

# Test 2: Upload file larger than 5MB
# → Frontend rejects before calling the API
# Expected: message "File too large. Maximum 5MB."

# Test 3: Upload a fake PDF (rename a .txt file to .pdf)
aws s3 cp fake.txt s3://hireflow-quarantine-226593872610/web/job-001/fake.pdf \
  --content-type application/pdf
# → FileValidator reads magic bytes → '%PDF' not found → FAIL
# → SNS alert sent to HR email
# Expected logs: [WARN] Validation FAIL — MIME invalid

# Test 4: Upload the same file twice (duplicate SHA-256)
# Note: SHA-256 is only written to DynamoDB after Lambda Extract completes
# (the next stage in the pipeline — see Workshop 1).
# If Extract has not yet run, the second upload will PASS FileValidator normally.
# Once Extract has run and stored the hash, subsequent uploads of the same file are blocked.
# → FileValidator queries DynamoDB GSI sha256_hash-index → hash found → FAIL
# Expected logs: [WARN] Validation FAIL — Duplicate file (SHA-256: ...)
```

### Presigned URL expiry test

```bash
# Get a presigned URL
curl "https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod/apply/presigned-url?job_id=job-001&file_name=cv.pdf"

# Wait 15 minutes, then attempt upload
curl -X PUT "<presigned_url>" \
  -H "Content-Type: application/pdf" \
  --data-binary @cv.pdf
# Expected: HTTP 403 AccessDenied — URL has expired
```




