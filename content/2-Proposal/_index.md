---

title: "Proposal"

date: 2026-07-10
weight: 2
chapter: false
pre: " <b>2.</b> "
---
# HireFlow AI — Serverless Recruitment Platform

### 1. Executive Summary

HireFlow AI is an automated candidate CV screening platform for HR departments, replacing repetitive manual tasks with an AWS Serverless platform integrated with artificial intelligence. The system supports CV intake from multiple channels (web form, email, direct upload), automatically extracts text via Amazon Textract, scores candidates using a large language model (LLM), and presents a ranked list on an HR Dashboard. The platform handles up to 10–15 open job postings and processes up to 200 CVs/day simultaneously, with scalability to match actual demand.

### 2. Problem Statement

 **Current Problem**

Current recruitment relies on manual operations: receiving CVs via email, downloading and reading each file, copying information to spreadsheets, cross-referencing with job descriptions (JD), and manually scoring candidates. Each CV takes 8–12 minutes to process; with 100 applications, an HR staff member needs approximately 15–20 hours of continuous work just for initial screening. Evaluations are inconsistent across candidates, potential resumes are easily overlooked, and there is no audit trail.

**Solution**

The platform uses Amazon S3 as a quarantine storage zone to receive raw CVs, AWS Lambda with Amazon Textract to extract text from PDF and DOCX, Amazon SQS to decouple processing stages, AWS Lambda with a large language model (LLM) to score candidates against the JD, and Amazon DynamoDB to store structured data. The React HR Dashboard is hosted on Amazon S3 distributed via CloudFront and secured with Amazon Cognito. The batch processing pipeline is fully automated, returning results within 2–3 minutes per CV.

**Benefits and ROI**

The solution cuts initial screening time by 80% (from 10 minutes to under 2 minutes per CV), ensures consistent evaluation via unified LLM scoring criteria, and creates a complete history for future auditing and analysis. Infrastructure cost is estimated under $5 USD/month for 200 CVs/day (per AWS Pricing Calculator), totalling under $60 USD/year. The break-even period is estimated at under 6 months based on saved HR man-hours.

### 3. Solution Architecture

**Overview**

HireFlow AI adopts a queue-based AWS Serverless architecture to ensure scalability, fault tolerance, and decoupling between stages. Candidates upload CVs via a presigned URL directly to the S3 Quarantine bucket, bypassing API Gateway to reduce load and exceed the 6 MB payload limit. After passing the format validation layer, CVs are moved to the S3 Clean bucket and queued into the extraction pipeline.

**Architecture Diagram**

HireFlow AI Architecture Diagram

**AWS Services Used**

- **Amazon S3 (2 buckets):** Quarantine zone receives raw files; Clean zone stores validated files for processing.
- **AWS Lambda (7 functions):** File-validator, Extract, Extract-complete, Score, Save-and-notify, Candidates, JD-management, Get-presigned-url.
- **Amazon API Gateway:** HTTP API connecting React SPA to Lambda, with Cognito authorizer.
- **Amazon SQS (2 queues):** Score-queue buffers before scoring; Save-queue buffers before saving and notifying.
- **Amazon Textract:** Extracts text and table structure from CV PDF/DOCX.
- **Amazon DynamoDB (2 tables):** Candidates stores scored profiles; Jobs stores job postings + JD + scoring criteria.
- **Amazon Cognito:** User pool for HR (5 accounts), separate pool for candidates (optional).
- **Amazon SNS:** Textract notification topic, HR alert topic for pipeline failure emails.
- **Amazon CloudFront + S3 Hosting:** Distributes the HR Dashboard (React SPA).
- **AWS Secrets Manager:** Stores LLM provider API key.
- **AWS KMS:** Encrypts data in S3, DynamoDB, and SQS.

**Component Design**

- **Zone 1 — Intake Layer:** Candidate requests a presigned URL from Lambda `get-presigned-url`, then uploads the file directly to S3 Quarantine via HTTPS PUT.
- **Zone 2 — Security Layer:** EventBridge rule catches S3 ObjectCreated events at the `web/` prefix; Lambda `file-validator` checks MIME (magic bytes), size ≤ 5 MB, SHA-256 dedup, then copies to Clean and deletes from Quarantine.
- **Zone 3 — Processing Layer:** Lambda `extract` reads the file from Clean, calls Textract async, stores JobId; SNS Textract calls Lambda `extract-complete` to merge results, sends SQS message to the scoring queue.
- **AI Layer:** Lambda `score` receives SQS message, reads JD from DynamoDB, calls LLM (OpenAI-compatible endpoint), saves score and reasoning to DynamoDB, sends SQS message to the save queue.
- **Persistence & Notification Layer:** Lambda `save-and-notify` writes the final record to DynamoDB and sends a confirmation email to the candidate via SES.
- **Interface Layer:** HR Dashboard React SPA calls API Gateway → Lambda `candidates`/`jd-management` to read/write DynamoDB, protected by Cognito.



### 4. Technical Implementation

**Implementation Phases**
This project has two parts—setting up weather edge stations and building the weather platform—each following 4 phases:

- Build Theory and Draw Architecture: Research Raspberry Pi setup with ESP32 sensors and design the AWS serverless architecture (1 month pre-internship)
- Calculate Price and Check Practicality: Use AWS Pricing Calculator to estimate costs and adjust if needed (Month 1).
- Fix Architecture for Cost or Solution Fit: Tweak the design (e.g., optimize Lambda with Next.js) to stay cost-effective and usable (Month 2).
- Develop, Test, and Deploy: Code the Raspberry Pi setup, AWS services with CDK/SDK, and Next.js app, then test and release to production (Months 2-3).

**Technical Requirements**

- Weather Edge Station: Sensors (temperature, humidity, rainfall, wind speed), a microcontroller (ESP32), and a Raspberry Pi as the edge device. Raspberry Pi runs Raspbian, handles Docker for filtering, and sends 1 MB/day per station via MQTT over Wi-Fi.
- Weather Platform: Practical knowledge of AWS Amplify (hosting Next.js), Lambda (minimal use due to Next.js), AWS Glue (ETL), S3 (two buckets), IoT Core (gateway and rules), and Cognito (5 users). Use AWS CDK/SDK to code interactions (e.g., IoT Core rules to S3). Next.js reduces Lambda workload for the fullstack web app.



### 5. Timeline & Milestones

**Project Timeline**

- Pre-Internship (Month 0): 1 month for planning and old station review.
- Internship (Months 1-3): 3 months.
  - Month 1: Study AWS and upgrade hardware.
  - Month 2: Design and adjust architecture.
  - Month 3: Implement, test, and launch.
- Post-Launch: Up to 1 year for research.



### 6. Budget Estimation

You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01).  
Or you can download the [Budget Estimation File](../attachments/budget_estimation.pdf).

### Infrastructure Costs

- AWS Services:
  - AWS Lambda: $0.00/month (1,000 requests, 512 MB storage).
  - S3 Standard: $0.15/month (6 GB, 2,100 requests, 1 GB scanned).
  - Data Transfer: $0.02/month (1 GB inbound, 1 GB outbound).
  - AWS Amplify: $0.35/month (256 MB, 500 ms requests).
  - Amazon API Gateway: $0.01/month (2,000 requests).
  - AWS Glue ETL Jobs: $0.02/month (2 DPUs).
  - AWS Glue Crawlers: $0.07/month (1 crawler).
  - MQTT (IoT Core): $0.08/month (5 devices, 45,000 messages).

Total: $0.7/month, $8.40/12 months

- Hardware: $265 one-time (Raspberry Pi 5 and sensors).



### 7. Risk Assessment



#### Risk Matrix

- Network Outages: Medium impact, medium probability.
- Sensor Failures: High impact, low probability.
- Cost Overruns: Medium impact, low probability.



#### Mitigation Strategies

- Network: Local storage on Raspberry Pi with Docker.
- Sensors: Regular checks and spares.
- Cost: AWS budget alerts and optimization.



#### Contingency Plans

- Revert to manual methods if AWS fails.
- Use CloudFormation for cost-related rollbacks.



### 8. Expected Outcomes



#### Technical Improvements:

Real-time data and analytics replace manual processes.  
Scalable to 10-15 stations.

#### Long-term Value

1-year data foundation for AI research.  
Reusable for future projects.