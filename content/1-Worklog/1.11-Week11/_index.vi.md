---
title: "Worklog Tuần 11"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b>1.11.</b> "
---

### Mục tiêu tuần 11:

- Kiểm thử toàn diện với 10 CV mẫu đa dạng, đảm bảo tất cả edge case được xử lý đúng
- Thiết lập monitoring CloudWatch và tối ưu chi phí hệ thống

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                                                                                              |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Kiểm thử 5 CV: CV chuẩn, CV thiếu kinh nghiệm, PDF scan chất lượng thấp, file giả extension, CV trùng lặp. Ghi nhận kết quả từng case                                                                       | 29/06/2026   | 29/06/2026      |                                                                                                                                                                                                                                                                                                                             |
| 3   | Kiểm thử thêm 5 CV: DOCX, DOC format cũ, CV tiếng Anh, CV nhiều bảng biểu, CV 1 trang. Verify score nhất quán cùng JD + rubric                                                                              | 30/06/2026   | 30/06/2026      |                                                                                                                                                                                                                                                                                                                             |
| 4   | Làm lab Cost and Performance Analysis với AWS Glue và Amazon Athena: tìm hiểu cách phân tích chi phí và hiệu năng hệ thống. CloudWatch Alarms: DLQ depth > 0, Lambda error rate > 5%. Verify KMS encryption | 01/07/2026   | 01/07/2026      | [Cost and Performance Analysis with AWS Glue and Amazon Athena :: Cost and Performance Analysis with AWS Glue and Amazon Athena](https://000040.awsstudygroup.com/)[AWS Key Management Service Documentation](https://docs.aws.amazon.com/kms/)[Amazon CloudWatch Documentation](https://docs.aws.amazon.com/cloudwatch/) |
| 5   | Fix bugs phát hiện qua test: `UPDATE_ROLLBACK_FAILED` SAM dùng `continue-update-rollback`, thiếu IAM permission một số Lambda. Tối ưu Lambda timeout và memory                                              | 02/07/2026   | 02/07/2026      |                                                                                                                                                                                                                                                                                                                             |
| 6   | AWS Cost Explorer: xem chi phí thực tế 2 tuần test. Tối ưu S3 lifecycle, Lambda config. Chuẩn bị checklist deploy production                                                                                | 03/07/2026   | 03/07/2026      |                                                                                                                                                                                                                                                                                                                             |




### Kết quả đạt được tuần 11:

- Kiểm thử 10 CV: tất cả pass đúng flow, score nhất quán
- Monitoring: CloudWatch Alarms + Logs Insights hoàn chỉnh
- Chi phí thực tế dưới dự kiến, bugs đã được fix

