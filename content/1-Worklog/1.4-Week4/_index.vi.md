---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 1
chapter: false
pre: " <b>1.4.</b> "
---

### Mục tiêu tuần 4:

- Nắm vững S3, IAM Permission Boundary — kiến thức cốt lõi cho upload flow và bảo mật
- Deploy được SAM template skeleton với đầy đủ infrastructure resources

### Các công việc cần triển khai trong tuần này:


| Thứ | Công việc                                                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Học Amazon S3 chi tiết: storage classes, versioning, lifecycle policy, bucket policy, static website hosting. Làm lab S3: tạo bucket, upload object, static hosting                                                 | 11/05/2026   | 11/05/2026      | [hSTART WITH AMAZON S3 :: Start with Amazon S3](https://000009.awsstudygroup.com/)[Amazon Simple Storage Service Documentation](https://docs.aws.amazon.com/s3/)                                                                           |
| 3   | S3 nâng cao: CORS, Presigned URL (generate bằng boto3 SigV4), SSE-KMS encryption. Làm lab S3 Storage Performance: prefix partitioning, throughput optimization                                                      | 12/05/2026   | 12/05/2026      | [Storage Performance Lab :: STORAGE PERFORMANCE LAB](https://000068.awsstudygroup.com/)[Amazon Simple Storage Service Documentation](https://docs.aws.amazon.com/s3/)                                                                      |
| 4   | Làm lab IAM Permission Boundary: thực hành giới hạn quyền tối đa cho IAM user/role. Thiết kế IAM roles least-privilege cho từng Lambda function trong project                                                       | 13/05/2026   | 13/05/2026      | [hLIMITATION OF USER RIGHTS WITH IAM PERMISSION BOUNDARY :: LIMITATION OF USER RIGHTS WITH IAM PERMISSION BOUNDARY](https://000011.awsstudygroup.com/)[AWS Identity and Access Management Documentation](https://docs.aws.amazon.com/iam/) |
| 5   | Học AWS SAM: `template.yaml` Globals, Lambda execution role. Viết SAM template skeleton: S3 buckets, DynamoDB tables, SQS queues (ScoreQueue + SaveQueue + DLQs), KMS key, SNS topics, Lambda functions placeholder | 14/05/2026   | 14/05/2026      | [AWS Serverless Application Model (AWS SAM) Documentation](https://docs.aws.amazon.com/serverless-application-model/)                                                                                                                       |


### Kết quả đạt được tuần 4:

- SAM skeleton deploy thành công: S3, DynamoDB, SQS, SNS, KMS tất cả tạo đúng
- Hiểu sâu S3 CORS và Presigned URL — kiến thức trực tiếp dùng cho project

