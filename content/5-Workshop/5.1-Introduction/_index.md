---
title : "Introduction"
date : 2026-07-10 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

This workshop guides you through building the complete candidate-side flow in the HireFlow AI system — from when the web page is loaded via CloudFront, the candidate selects a CV file, to when the file is uploaded directly to S3 and passes through the automated security validation layer.

**Target Architecture:** Completely decouple the upload flow from the backend processing pipeline. The CV file itself never passes through Lambda or API Gateway — the candidate calls the API to **obtain a presigned URL** (Lambda GetPresignedUrl), then the browser uses that URL to PUT the file directly to S3 without going through any Lambda. This bypasses API Gateway's 6MB payload limit and eliminates Lambda from the data path entirely.

**Flow Overview (steps 1–7 in the architecture):**

```
1. Load Web       → CloudFront serves Candidate App (React/Vite)
2. Fetch Assets   → Browser loads JS/CSS from S3 via CloudFront
3. HTTPS Request  → Candidate submits form → calls API Gateway
4. Get Presign    → Lambda GetPresignedUrl returns an S3 PUT URL
5. Upload to S3   → Browser PUTs file directly to S3 Quarantine
6. Check Hash     → Lambda FileValidator checks magic bytes + SHA-256
7. Move to Clean  → File PASS → copy to S3 Clean, delete from Quarantine
```

**Why this design:**
- API Gateway has a 6MB payload limit — CVs are typically 1–5MB, making direct Lambda upload impractical
- Presigned URL allows the browser to upload directly to S3 with a time-limited signature (15 minutes)
- S3 Quarantine acts as an isolation zone: unvalidated files never reach the processing pipeline
- Separation of concerns: Lambda only generates the URL and validates metadata, never reads large binary files