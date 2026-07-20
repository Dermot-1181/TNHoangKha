---
title : "Giới thiệu"
date : 2026-07-10 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---


Workshop này hướng dẫn xây dựng toàn bộ luồng phía ứng viên trong hệ thống HireFlow AI — từ khi trang web được tải qua CloudFront, ứng viên chọn file CV, cho đến khi file được upload trực tiếp lên S3 và qua lớp kiểm tra bảo mật tự động.

**Kiến trúc đích:** Tách biệt hoàn toàn luồng upload khỏi backend xử lý. File CV không đi qua Lambda hay API Gateway — ứng viên gọi API để **lấy presigned URL** (Lambda GetPresignedUrl), sau đó browser dùng URL đó để PUT file thẳng lên S3 mà không qua bất kỳ Lambda nào. Cách này vượt qua giới hạn 6MB payload của API Gateway và giảm tải hoàn toàn cho Lambda.

**Tổng quan luồng (bước 1–7 trong architecture):**

```
1. Load Web       → CloudFront phân phối Candidate App (React/Vite)
2. Fetch Assets   → Browser tải JS/CSS từ S3 qua CloudFront
3. HTTPS Request  → Ứng viên submit form → gọi API Gateway
4. Get Presign    → Lambda GetPresignedUrl trả về S3 PUT URL
5. Upload to S3   → Browser PUT file thẳng lên S3 Quarantine
6. Check Hash     → Lambda FileValidator kiểm tra magic bytes + SHA-256
7. Upload to S3   → File PASS → copy sang S3 Clean, xóa Quarantine
```

**Lý do thiết kế theo mô hình này:**
- API Gateway có giới hạn payload 6MB — CV thường 1–5MB, không thể truyền qua Lambda
- Presigned URL cho phép browser upload thẳng lên S3 với signature có thời hạn (15 phút)
- S3 Quarantine đóng vai trò vùng cách ly: file chưa qua kiểm tra không bao giờ đến pipeline xử lý
- Tách biệt concerns: Lambda không cần đọc binary file lớn, chỉ tạo URL và validate metadata