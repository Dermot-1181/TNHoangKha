---
title: "Week 5 Worklog"

date: 2026-05-18
weight: 1
chapter: false
pre: " **1.5.** "
---
### Week 5 Objectives:

- Understand the AWS Lambda execution model, cold starts, and how to integrate with API Gateway.
- Finalize the entire infrastructure setup (CORS, lifecycle policies, KMS, Secrets Manager, and EventBridge), making it fully ready for implementation.

---

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                                                                                                                                                                                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Learn AWS Lambda in detail: Python 3.12 runtime, cold starts, execution roles, environment variables, and CloudWatch Logs. Write the first Lambda function and integrate it with an API Gateway REST endpoint.                                                                                                                                                         | 18/05/2026 | 18/05/2026      | [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/) [Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)                                                                   |
| 3   | Complete the Serverless with AWS SAM lab: deploy a complete Lambda + API Gateway stack; understand the workflow of `sam build` → `sam deploy` → testing. Configure S3 CORS for the Quarantine bucket and set up S3 lifecycle policies for the project.                                                                                                                 | 19/05/2026 | 19/05/2026      | [Serverless - Deploying applications with SAM :: SERVERLESS - DEPLOYING APPLICATIONS WITH SAM](https://000080.awsstudygroup.com/) [Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/) |
| 4   | Complete the Amazon Macie lab: create a scan job on S3 to detect PII (Personally Identifiable Information) and analyze findings. Configure the Secrets Manager secret `hireflow/gemini-api-key`. Ensure Lambda retrieves the key during cold start and caches it for warm invocations.                                                                                 | 20/05/2026 | 20/05/2026      | [Discover sensitive data present in S3 bucket using Amazon Macie :: Discover sensitive data present in S3 bucket using Amazon MacieAWS Secrets Manager Documentation](https://000090.awsstudygroup.com/)      |
| 6   | Fine-tune the EventBridge rule `hireflow-quarantine-upload` with an S3 `ObjectCreated` event pattern targeting the `web/` prefix. Understand why this rule is created outside of SAM (to avoid an S3 ↔ Lambda Permission circular dependency). Review the complete SAM template, standardize naming conventions with a `hireflow-*` prefix, and verify KMS encryption. | 22/05/2026 | 22/05/2026      | [Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/)                                                                                                                                  |

### Week 5 Achievements:

- Finalized and perfected the SAM template: CORS, lifecycle rules, KMS, Secrets Manager, and EventBridge configurations are all accurately deployed.
- Gained a deep understanding of the AWS Lambda execution model and strategies to optimize cold starts.
- Completed the foundational infrastructure, rendering the environment fully ready for active codebase implementation.

