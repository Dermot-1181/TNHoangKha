---
title : "Implementation Steps"
date : 2026-07-10 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

Phần này triển khai toàn bộ luồng phía ứng viên (Candidate App) trong HireFlow AI — từ lúc trang web được tải qua CloudFront, ứng viên chọn và upload file CV, cho đến khi file đi qua lớp kiểm tra bảo mật tự động và được lưu vào S3 Clean.

Kiến trúc dùng **presigned URL**: Candidate App gọi API lấy URL có chữ ký (Lambda GetPresignedUrl), sau đó browser PUT file thẳng lên S3 mà không qua Lambda hay API Gateway — giúp vượt giới hạn 6MB payload và giảm tải xử lý.


#### Nội dung

- [5.3.1-Initialize Candidate App (React + Vite)](./5.3.1-Initialize%20Candidate%20App%20(React%20+%20Vite)/_index.vi.md)
- [5.3.2 - Lambda GetPresignedUrl — Tạo S3 Upload URL](./5.3.2-Lambda%20GetPresignedUrl%20—%20generate%20S3%20upload%20URL/_index.vi.md)
- [5.3.3 - Candidate App — Upload 2 bước](./5.3.3-Candidate%20App%20—%20Two-step%20upload/_index.vi.md)
- [5.3.4 - Drag & Drop và UX Upload](./5.3.4-Drag%20&%20Drop%20and%20Upload%20UX/_index.vi.md)
- [5.3.5 - Lambda FileValidator — Kiểm tra bảo mật file](./5.3.5-Lambda%20FileValidator%20—%20Security%20Layer/_index.vi.md)
- [5.3.6 - Deploy Frontend lên S3 + CloudFront (OAC)](./5.3.6-Deploy%20Frontend%20to%20S3%20+%20CloudFront/_index.vi.md)
- [5.3.7 - Configure S3 CORS](./5.3.7-Configure%20S3%20CORS%20—%20required%20for%20browser%20upload/_index.vi.md)