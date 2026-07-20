---
title : "Các bước chuẩn bị"
date : 2026-07-10 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

- **Node.js 18+** and **npm** installed: `node --version`
- **AWS CLI** configured with credentials that have S3, Lambda, API Gateway permissions
- **AWS SAM CLI** version >= 1.90.0 (for deploying the Lambda backend)
- **Git** installed
- Stack `hireflow-prod` already deployed (API Gateway URL available from `Outputs.ApiUrl`) — see Workshop 1 for SAM stack deployment steps
- S3 Quarantine bucket created with EventBridge notifications enabled
- CloudFront distribution created (or S3 static hosting for dev)

**Verify setup:**

```bash
node --version
# v18.x.x or higher

npm --version
# 9.x.x or higher

aws sts get-caller-identity
# Confirm correct account and region

# Get API URL from SAM stack output
aws cloudformation describe-stacks \
  --stack-name hireflow-prod \
  --query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' \
  --output text
# https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod```