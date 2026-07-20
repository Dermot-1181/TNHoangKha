---

title: "Worklog Tuần 6"
date: 2026-05-25
weight: 1
chapter: false
pre: " **1.6.** "

---

### Mục tiêu tuần 6:

- Hoàn thành luồng upload CV end-to-end: Lambda GetPresignedUrl + Candidate App + FileValidator
- Xử lý đúng các edge case bảo mật: file giả extension, file trùng lặp

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2   | Implement Lambda `get_presigned_url`: validate job_id chặn path traversal, validate extension, generate presigned PUT URL SigV4 expires 900s. Deploy SAM, test curl thành công                         | 25/05/2026   | 25/05/2026      |                |
| 3   | Setup Candidate App (React + Vite): `npm create vite@latest`, cài axios, cấu hình `.env` VITE_API_BASE. Implement upload form: validate type PDF/DOCX/DOC và size ≤ 5MB client-side                    | 26/05/2026   | 26/05/2026      |                |
| 4   | Implement 2-step upload App.jsx: GET presigned URL → PUT file S3. Debug 403 Signature Mismatch: axios tự thêm `multipart/form-data`. Fix: convert `File → ArrayBuffer` + map Content-Type từ extension | 27/05/2026   | 27/05/2026      |                |
| 5   | - Tìm hiểu EC2 cơ bản: &emsp; + Instance types &emsp; + AMI &emsp; + EBS &emsp; + ... - Các cách remote SSH vào EC2 - Tìm hiểu Elastic IP                                                              | 28/05/2026   | 28/05/2026      |                |
| 6   | Hoàn thiện FileValidator: copy S3 Clean giữ metadata, xóa Quarantine, SNS alert HR khi fail. Implement drag & drop, progress bar, success/error state `key={status}`. Test all cases                   | 29/05/2026   | 29/05/2026      |                |




### Kết quả đạt được tuần 6:

- Upload CV từ browser lên S3 Quarantine thành công qua presigned URL
- FileValidator phát hiện đúng file giả magic bytes và file trùng SHA-256
- Candidate App UX hoàn chỉnh: drag & drop, progress realtime, error handling

