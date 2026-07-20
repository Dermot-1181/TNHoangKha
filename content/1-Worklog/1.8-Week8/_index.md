---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 1
chapter: false
pre: " **1.8.** "
---

### Week 8 Objectives:

- Integrate the LLM API to score CVs against the JD (Job Description) and scoring rubric.
- Build a multi-layer defense mechanism against prompt injection attacks.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                                                                                           | Start Date | Completion Date | Reference Material                                                                                                                                                  |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Learn OpenAI-compatible APIs: request/response formats, configuring `response_format: json_object`, and setting `temperature=0`. Implement part 1 of the `Score` Lambda function: read OCR text from S3, fetch the JD and rubric from DynamoDB, and retrieve the API key from Secrets Manager.                                                 | 08/06/2026 | 08/06/2026      | [AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/)[Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/)             |
| 4   | Research prompt injection attacks. Design sentinel guards (`<<<TAG_START/END>>>`) and write system prompt defenses. Implement the `_sanitize_untrusted` helper method along with output sanitization logic.                                                                                                                                    | 10/06/2026 | 10/06/2026      |                                                                                                                                                                     |
| 5   | Implement LLM retry logic: enforce 3 retry attempts with exponential backoff, targeting only 5xx errors and 429 rate limits. Parse the returned JSON, validate it against the schema, and send the payload to the SQS `SaveQueue`. Test DLQ (Dead Letter Queue) routing after 3 failed attempts, and configure a CloudWatch alarm for the DLQ. | 11/06/2026 | 11/06/2026      |                                                                                                                                                                     |
| 6   | Test the `Score` Lambda function with real CVs and JDs: verify that the JSON output complies with the schema and that `score_total` aggregates reasonably. Learn Cost & Performance Analysis using AWS Glue and Amazon Athena.                                                                                                                 | 12/06/2026 | 12/06/2026      | [Cost and Performance Analysis with AWS Glue and Amazon Athena :: Cost and Performance Analysis with AWS Glue and Amazon Athena](https://000040.awsstudygroup.com/) |




### Week 8 Achievements:

- Successfully integrated the `Score` Lambda function with the LLM API, yielding structured JSON scoring data that adheres perfectly to the required schema.
- Established strong prompt injection defenses using sentinel guards, input sanitization, and output validation.
- Validated that the retry mechanisms, DLQ routing, and CloudWatch alarm systems trigger and function exactly as intended.

