---

title: "Week 7 Worklog"
date: 2026-06-01
weight: 1
chapter: false
pre: " **1.7.** "

---
### Week 7 Objectives:

- Understand Amazon Textract asynchronous operations and correctly implement the polling workflow.
- Complete the `Extract` Lambda function to run end-to-end from the Clean S3 bucket to the SQS `ScoreQueue`.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                                                     |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------------------------------------------------------------------- |
| 2   | Learn Amazon Textract: synchronous vs. asynchronous processing and when to use `start_document_text_detection`. Test the Textract console using sample PDFs. Understand SNS callbacks and EventBridge completion events.                                                                                                                                    | 01/06/2026 | 01/06/2026      | [Amazon Textract Documentation](https://docs.aws.amazon.com/textract/) |
| 3   | Implement part 1 of the `Extract` Lambda function: use `head_object` to retrieve metadata, copy the file to the CV Files S3 bucket, and execute a DynamoDB `put_item` into the `Candidates` table with `status=new` and a `candidate_id=uuid4`.                                                                                                             | 02/06/2026 | 02/06/2026      |                                                                        |
| 4   | Implement part 2 of the `Extract` Lambda function: trigger Textract async via `start_document_text_detection`, build a 60 $\times$ 5-second polling loop, and save the raw text to the OCR Results S3 bucket. Validate that the text length is $\ge$ 200 characters.                                                                                        | 03/06/2026 | 03/06/2026      |                                                                        |
| 5   | Fix the circular dependency between the Clean S3 bucket and the `Extract` Lambda function by using an ARN literal. Implement idempotency via a `_check_already_processed` method. Implement the `extract_complete` Lambda logic. Test the entire end-to-end flow: upload $\rightarrow$ Extract $\rightarrow$ raw text in S3 $\rightarrow$ SQS `ScoreQueue`. | 04/06/2026 | 04/06/2026      |                                                                        |

### Week 7 Achievements:

- Successfully ran the `Extract` Lambda end-to-end: Clean S3 bucket $\rightarrow$ Textract OCR $\rightarrow$ SQS `ScoreQueue`.
- Correctly implemented idempotency to safely handle SQS at-least-once delivery.
- Gained a solid understanding of circular dependency patterns in AWS SAM and how to fix them.

