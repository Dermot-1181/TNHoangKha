---
title: "Workshop"
date: 2026-07-10
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Build a CV with automated pipeline processing — HireFlow AI


![](/images/2-Proposal/Proposal.png)


#### Overview

This workshop walks through building the entire HireFlow AI system on AWS Serverless — from candidates uploading their CV through the Candidate App, the file being security-checked (Lambda FileValidator), text extraction with **Amazon Textract** (Lambda Extract), scoring with **Extend AI** (Lambda Score), and finally the result being saved to **DynamoDB** (Candidate, JD tables) and notified via email (Lambda Save & Notify).

**Architecture:** A queue-based serverless pipeline — each processing stage (Extract → Score → Save&Notify) is decoupled with **SQS**, ensuring scalability and fault-tolerance: a failing Lambda doesn't bring down the whole pipeline, and messages retry automatically. There's no persistent server — everything runs on Lambda, auto-scaling with the volume of CVs submitted. Access control is managed via **IAM**, data is encrypted with **KMS**, and operations are monitored with **CloudWatch**.




#### Contents

1. [Introduction](5.1-Introduction/_index.md)
2. [Prerequisite](5.2-Prerequiste/_index.md)
3. [Implementation Steps](5.3-Implementation%20Steps/_index.md)
4. [Validation & Analysis](5.4-Validation%20&%20Analysis/_index.md)
5. [Cleanup](5.5-Cleanup/_index.md)
6. [Challenges & Future Direction](5.6-Challenges%20&%20Future%20Direction/_index.md)
7. [5.7-Product Demonstration](5.7-Product%20Demonstration/_index.md)