---

title: "Week 11 Worklog"
date: 2026-06-29
weight: 2
chapter: false
pre: " **1.11.** "

---

### Week 11 Objectives:

- Conduct comprehensive testing using 10 diverse sample CVs, ensuring all edge cases are handled correctly.
- Set up CloudWatch monitoring and optimize overall system costs.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                         | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                         |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2   | Test the first batch of 5 CVs: standard CV, inexperienced candidate CV, low-quality scanned PDF, spoofed file extension, and duplicate CV submission. Record the execution results for each case.                                                                            | 29/06/2026 | 29/06/2026      |                                                                                                                                                                                                                                            |
| 3   | Test the second batch of 5 CVs: DOCX format, legacy DOC format, English-language CV, layout with heavy tables/charts, and a single-page CV. Verify that the generated scores remain consistent against the same JD and rubric.                                               | 30/06/2026 | 30/06/2026      |                                                                                                                                                                                                                                            |
| 4   | Complete the Cost and Performance Analysis with AWS Glue and Amazon Athena lab to learn methods for analyzing system architecture costs and performance. Configure CloudWatch Alarms: set triggers for DLQ depth > 0 and Lambda error rate > 5%. Verify KMS encryption keys. | 01/07/2026 | 01/07/2026      | [Cost and Performance Analysis with AWS Glue and Amazon Athena :: Cost and Performance Analysis with AWS Glue and Amazon AthenaAWS Key Management Service DocumentationAmazon CloudWatch Documentation](https://000040.awsstudygroup.com/) |
| 5   | Fix bugs identified during testing: resolve AWS SAM `UPDATE_ROLLBACK_FAILED` errors using `continue-update-rollback`, and fix missing IAM permissions on certain Lambda functions. Optimize Lambda execution timeouts and memory allocations.                                | 02/07/2026 | 02/07/2026      |                                                                                                                                                                                                                                            |
| 6   | Analyze AWS Cost Explorer data to review actual expenditures from the past 2 weeks of testing. Optimize S3 lifecycle rules and Lambda configurations to reduce footprint. Prepare the production deployment checklist.                                                       | 03/07/2026 | 03/07/2026      |                                                                                                                                                                                                                                            |




### Week 11 Achievements:

- Completed comprehensive testing across 10 distinct CV profiles: all files processed successfully through the workflow with consistent, reliable scoring.
- Established robust monitoring infrastructure: fully integrated CloudWatch Alarms and Logs Insights queries.
- Maintained live operational costs well below initial budget projections and successfully patched all system bugs.

