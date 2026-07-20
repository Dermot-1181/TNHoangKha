---
title: "Week 4 Worklog"
date: 2026-05-11
weight: 1
chapter: false
pre: " <b>1.4.</b> "
---

### Week 4 Objectives:

- Master Amazon S3 and IAM Permission Boundaries — core knowledge required for the application's upload workflow and security.
- Successfully deploy the AWS SAM template skeleton containing all infrastructure resources.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                              | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                          |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Learn Amazon S3 in depth: storage classes, versioning, lifecycle policies, bucket policies, and static website hosting. Complete the S3 lab: create buckets, upload objects, and configure static hosting.                                        | 11/05/2026 | 11/05/2026      | [hSTART WITH AMAZON S3 :: Start with Amazon S3](https://000009.awsstudygroup.com/) [Amazon Simple Storage Service Documentation](https://docs.aws.amazon.com/s3/)                                                                           |
| 3   | **Advanced S3:** • Focus on CORS, Presigned URLs (generated via `boto3` SigV4), and SSE-KMS encryption. • Complete the S3 Storage Performance lab: prefix partitioning and throughput optimization.                                           | 12/05/2026 | 12/05/2026      | [Storage Performance Lab :: STORAGE PERFORMANCE LAB](https://000068.awsstudygroup.com/) [Amazon Simple Storage Service Documentation](https://docs.aws.amazon.com/s3/)                                                                      |
| 4   | Complete the IAM Permission Boundary lab: practice enforcing the maximum allowable permissions for an IAM user/role. Design least-privilege IAM roles tailored for each Lambda function in the project.                                           | 13/05/2026 | 13/05/2026      | [hLIMITATION OF USER RIGHTS WITH IAM PERMISSION BOUNDARY :: LIMITATION OF USER RIGHTS WITH IAM PERMISSION BOUNDARY](https://000011.awsstudygroup.com/) [AWS Identity and Access Management Documentation](https://docs.aws.amazon.com/iam/) |
| 5   | Learn AWS SAM: focus on `template.yaml` Globals and Lambda execution roles. Write the SAM template skeleton: S3 buckets, DynamoDB tables, SQS queues (`ScoreQueue` + `SaveQueue` + DLQs), KMS keys, SNS topics, and placeholder Lambda functions. | 14/05/2026 | 14/05/2026      | [AWS Serverless Application Model (AWS SAM) Documentation](https://docs.aws.amazon.com/serverless-application-model/)                                                                                                                       |




### Week 4 Achievements:

- Successfully deployed the SAM skeleton: S3, DynamoDB, SQS, SNS, and KMS resources were all created accurately.
- Gained a deep understanding of S3 CORS and Presigned URLs — knowledge that applies directly to the implementation of the project.

