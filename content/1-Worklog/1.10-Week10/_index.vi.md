---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b>1.10.</b> "
---

### Mục tiêu tuần 10:

- Xây dựng HR Dashboard hoàn chỉnh: CRUD Job, xem ứng viên theo điểm, tải CV, cập nhật status
- Hoàn thiện tất cả Lambda APIs cho phía HR

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------- |
| 2   | Setup HR Dashboard React + Vite. Implement Lambda `jd_management`: CRUD POST/GET/PUT/DELETE `/dashboard/jobs`. Test API curl                                                                            | 22/06/2026   | 22/06/2026      |                |
| 3   | Implement Lambda `candidates`: GET list query GSI `job_id-score_total-index`, GET detail + presigned download URL. Implement Lambda `random_cv`                                                         | 23/06/2026   | 23/06/2026      |                |
| 4   | Implement `PUT /dashboard/candidates/{id}` update status shortlisted/rejected/hired. HR Dashboard UI: Jobs list, form tạo/sửa Job với JD text + rubric. Candidates list filter theo job, sort theo điểm | 24/06/2026   | 24/06/2026      |                |
| 5   | Trang chi tiết ứng viên: summary, flags, confidence, download CV, update status. Test toàn bộ dashboard với dữ liệu thật. Fix lỗi UI và API response format                                             | 25/06/2026   | 25/06/2026      |                |




### Kết quả đạt được tuần 10:

- HR Dashboard hoàn chỉnh: CRUD Job, xem ứng viên, tải CV, cập nhật status
- Tất cả Lambda APIs đúng với DynamoDB
- UI hiển thị đầy đủ thông tin scoring từ LLM

