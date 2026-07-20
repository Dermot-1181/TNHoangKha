---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b>1.9.</b> "
---

### Week 9 Objectives:

- Complete the `SaveAndNotify` Lambda function: save results to the database and send notification emails to candidates via Amazon SES.
- Perform the first end-to-end pipeline test using real CVs, ensuring the entire workflow runs successfully in less than 3 minutes.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                                                                                                                                      | Start Date | Completion Date | Reference Material                                                                                                                                   |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | Learn Amazon SES: verify sandbox emails, send HTML and plain text test emails using `boto3`, and understand sending limits and bounce handling. Implement the `save_notify` Lambda logic using an idempotent DynamoDB `UpdateItem` operation.                                                                                                                                             | 15/06/2026 | 15/06/2026      | [Amazon Simple Email Service Documentation](https://docs.aws.amazon.com/ses/)[Amazon DynamoDB Documentation](https://docs.aws.amazon.com/dynamodb/) |
| 4   | Implement the SES confirmation email template for candidates. Set up an SNS alert to notify HR when `confidence=low` or if system flags are triggered. Test the `Score` $\rightarrow$ `Save` pipeline segment: verify the database records in DynamoDB, check the email inbox, and confirm SNS alerts fire under correct conditions.                                                      | 17/06/2026 | 17/06/2026      |                                                                                                                                                      |
| 6   | Conduct full end-to-end pipeline testing: upload a real CV $\rightarrow$ `FileValidator` $\rightarrow$ `Extract` $\rightarrow$ `Score` $\rightarrow$ `Save&Notify` $\rightarrow$ final email delivery. Measure execution time to guarantee processing takes less than 3 minutes per CV, perform concurrent load testing with 3 CVs simultaneously, and verify zero race conditions occur. | 19/06/2026 | 19/06/2026      |                                                                                                                                                      |




### Week 9 Achievements:

- Successfully ran the core end-to-end pipeline: upload → score → email delivery completed in less than 3 minutes.
- DynamoDB records are fully populated, with candidate emails and HR SNS alerts delivering under the exact correct conditions.
- Verified that the architecture experiences zero race conditions when handling multiple CV submissions concurrently.

