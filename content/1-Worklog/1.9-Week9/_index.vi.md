---
title: "Worklog Tuần 9"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b>1.9.</b> "
---

### Mục tiêu tuần 9:

- Hoàn thành Lambda SaveAndNotify: lưu kết quả DB + gửi email SES cho ứng viên
- Kiểm thử pipeline end-to-end lần đầu với CV thật, đảm bảo toàn bộ chạy đúng < 3 phút

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                       |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Học Amazon SES: verify email sandbox, gửi test email HTML + plain text boto3. Tìm hiểu sending limits, bounce handling. Implement Lambda `save_notify`: `UpdateItem` DynamoDB idempotent | 15/06/2026   | 15/06/2026      | [Amazon Simple Email Service Documentation](https://docs.aws.amazon.com/ses/)[Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/) |
| 4   | Implement SES email xác nhận cho ứng viên. SNS alert HR khi confidence=`low` hoặc có flags. Test pipeline Score → Save: verify DynamoDB record, email inbox, SNS alert đúng điều kiện    | 17s/06/2026  | 17/06/2026      |                                                                                                                                                      |
| 6   | Test pipeline end-to-end: upload CV thật → FileValidator → Extract → Score → Save&Notify → email. Đo thời gian xử lý < 3 phút/CV, test song song 3 CV, verify không race condition       | 19/06/2026   | 19/06/2026      |                                                                                                                                                      |




### Kết quả đạt được tuần 9:

- Core pipeline end-to-end: upload → score → email < 3 phút
- DynamoDB record đầy đủ, email ứng viên và SNS HR đều đúng
- Không race condition khi xử lý nhiều CV cùng lúc

