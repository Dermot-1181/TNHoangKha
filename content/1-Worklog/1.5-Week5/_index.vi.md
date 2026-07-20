---
title: "Worklog Tuần 5"
date: 2026-05-18
weight: 1
chapter: false
pre: " **1.5.** "
---

### Mục tiêu tuần 5:

- Hiểu Lambda execution model, cold start, cách tích hợp với API Gateway
- Hoàn thiện toàn bộ infrastructure: CORS, lifecycle, KMS, Secrets Manager, EventBridge sẵn sàng cho implement

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                           |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Học Lambda chi tiết: runtime Python 3.12, cold start, execution role, environment variables, CloudWatch Logs. Viết Lambda đầu tiên + tích hợp API Gateway REST endpoint                                                                                          | 18/05/2026   | 18/05/2026      | [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/) [Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)                                                                                                              |
| 3   | Làm lab Serverless với AWS SAM: deploy Lambda + API Gateway hoàn chỉnh, hiểu flow `sam build` → `sam deploy` → test. Cấu hình S3 CORS Quarantine bucket và S3 lifecycle policies cho project                                                                     | 19/05/2026   | 19/05/2026      | [Serverless - Deploying applications with SAM :: SERVERLESS - DEPLOYING APPLICATIONS WITH SAM](https://000080.awsstudygroup.com/)[Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)                                            |
| 4   | Làm lab Amazon Macie: tạo job scan S3 phát hiện PII, xem findings. Cấu hình Secrets Manager secret `hireflow/gemini-api-key`. Lambda đọc key khi cold start, cache cho warm invocation                                                                           | 20/05/2026   | 20/05/2026      | [Discover sensitive data present in S3 bucket using Amazon Macie :: Discover sensitive data present in S3 bucket using Amazon Macie](https://000090.awsstudygroup.com/)[AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/) |
| 6   | Tinh chỉnh EventBridge rule `hireflow-quarantine-upload`: event pattern S3 ObjectCreated prefix `web/`. Hiểu lý do tạo ngoài SAM (circular dependency S3 ↔ Lambda Permission). Review toàn bộ SAM template, chuẩn hóa naming `hireflow-*`, verify KMS encryption | 22/05/2026   | 22/05/2026      | [Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/)                                                                                                                                                                             |




### Kết quả đạt được tuần 5:

- SAM template hoàn thiện: CORS, lifecycle, KMS, Secrets Manager, EventBridge đều đúng
- Hiểu Lambda execution model và cách tối ưu cold start
- Infrastructure hoàn chỉnh, sẵn sàng bắt đầu implement

