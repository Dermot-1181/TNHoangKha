---

## title: "Worklog Tuần 8"

date: 2026-06-08
weight: 1
chapter: false
pre: " **1.8.** "

### Mục tiêu tuần 8:

- Tích hợp LLM API để chấm điểm CV theo JD + rubric
- Xây dựng cơ chế bảo vệ chống prompt injection multi-layer

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                      |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Học OpenAI-compatible API: request/response format, `response_format: json_object`, temperature=0. Implement Lambda `score` phần 1: đọc OCR text S3, lấy JD + rubric DynamoDB, đọc API key Secrets Manager | 08/06/2026   | 08/06/2026      | [AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/)[Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)             |
| 4   | Nghiên cứu prompt injection attack. Thiết kế sentinel guards `<<<TAG_START/END>>>`, viết system prompt guard. Implement `_sanitize_untrusted` + output sanitization                                        | 10/08/2025   | 10/08/2025      |                                                                                                                                                                     |
| 5   | LLM retry: exponential backoff 3 lần, chỉ retry 5xx + 429. Parse JSON, validate schema, gửi SQS SaveQueue. Test DLQ sau 3 fail. CloudWatch alarm DLQ                                                       | 11/08/2025   | 11/08/2025      |                                                                                                                                                                     |
| 6   | Test Lambda Score với CV + JD thật: verify JSON output đúng schema, score_total hợp lý. Học Cost & Performance Analysis với AWS Glue và Amazon Athena                                                      | 12/08/2025   | 112/08/2025     | [Cost and Performance Analysis with AWS Glue and Amazon Athena :: Cost and Performance Analysis with AWS Glue and Amazon Athena](https://000040.awsstudygroup.com/) |




### Kết quả đạt được tuần 8:

- Lambda Score gọi LLM thành công, JSON score đúng schema
- Prompt injection defense: sentinel guard + sanitize + validate output
- Retry + DLQ hoạt động, CloudWatch alarm bắn đúng

