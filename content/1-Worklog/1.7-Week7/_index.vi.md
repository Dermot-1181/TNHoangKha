---

## title: "Worklog Tuần 7"

date: 2026-06-01
weight: 1
chapter: false
pre: " **1.7.** "

### Mục tiêu tuần 7:

- Hiểu Textract async và implement đúng polling flow
- Hoàn thành Lambda Extract chạy end-to-end từ S3 Clean đến SQS ScoreQueue

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                         |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ---------------------------------------------------------------------- |
| 2   | Học Amazon Textract: sync vs async, khi nào dùng `start_document_text_detection`. Test Textract console với PDF mẫu. Hiểu SNS callback + EventBridge completion event                                                         | 01/06/2026   | 01/06/2026      | [Amazon Textract Documentation](https://docs.aws.amazon.com/textract/) |
| 3   | Implement Lambda `extract` phần 1: `head_object` lấy metadata, copy file sang S3 CV Files, `put_item` DynamoDB Candidates status=`new`, candidate_id=uuid4                                                                    | 02/06/2026   | 02/06/2026      |                                                                        |
| 4   | Implement Lambda `extract` phần 2: `start_document_text_detection` Textract async, polling loop 60×5s, lưu raw text S3 OCR Results. Validate length ≥ 200 ký tự                                                               | 03/06/2026   | 03/06/2026      |                                                                        |
| 5   | Fix circular dependency S3 Clean ↔ Lambda Extract (dùng ARN literal). Implement idempotency `_check_already_processed`. Implement Lambda `extract_complete`. Test end-to-end: upload → Extract → raw text S3 → SQS ScoreQueue | 04/06/2026   | 04/06/2026      |                                                                        |




### Kết quả đạt được tuần 7:

- Lambda Extract end-to-end: S3 Clean → Textract OCR → SQS ScoreQueue
- Idempotency đúng với SQS at-least-once delivery
- Hiểu và fix circular dependency pattern trong AWS SAM

