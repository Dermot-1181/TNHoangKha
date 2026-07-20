---
title : "Implementation Steps"
date : 2026-07-10
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---


This section implements the full candidate-facing flow in HireFlow AI — from loading the web app via CloudFront, through selecting and uploading a CV, to passing through the automated security validation layer and landing in the S3 Clean bucket.

The architecture uses **presigned URLs**: the Candidate App calls an API to get a signed URL (Lambda GetPresignedUrl), then the browser PUTs the file directly to S3 — bypassing Lambda and API Gateway entirely, avoiding the 6MB payload limit and offloading processing.



#### Content

- [5.3.1 - Initialize Candidate App (React + Vite)](./5.3.1-Initialize%20Candidate%20App%20(React%20+%20Vite)/_index.md)
- [5.3.2 - Lambda GetPresignedUrl — Generate S3 Upload URL](./5.3.2-Lambda%20GetPresignedUrl%20—%20generate%20S3%20upload%20URL/_index.md)
- [5.3.3 - Candidate App — Two-step Upload](./5.3.3-Candidate%20App%20—%20Two-step%20upload/_index.md)
- [5.3.4 - Drag & Drop and Upload UX](./5.3.4-Drag%20&%20Drop%20and%20Upload%20UX/_index.md)
- [5.3.5 - Lambda FileValidator — Security File Check](./5.3.5-Lambda%20FileValidator%20—%20Security%20Layer/_index.md)
- [5.3.6 - Deploy Frontend to S3 + CloudFront (OAC)](./5.3.6-Deploy%20Frontend%20to%20S3%20+%20CloudFront/_index.md)
- [5.3.7 - Configure S3 CORS](./5.3.7-Configure%20S3%20CORS%20—%20required%20for%20browser%20upload/_index.md)