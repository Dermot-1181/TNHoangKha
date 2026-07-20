---
title : "Prerequiste"
date : 2026-07-10 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

- **Node.js 18+** và **npm** đã cài đặt: `node --version`
- **AWS CLI** đã configure với credentials có quyền S3, Lambda, API Gateway
- **AWS SAM CLI** phiên bản >= 1.90.0 (để deploy Lambda backend)
- **Git** đã cài đặt
- Stack `hireflow-prod` đã được deploy (có sẵn API Gateway URL từ `Outputs.ApiUrl`) — xem Workshop 1 để biết cách deploy SAM stack
- S3 Quarantine bucket đã tạo với EventBridge notifications bật
- CloudFront distribution đã tạo (hoặc dùng S3 static hosting cho dev)

**Kiểm tra môi trường:**

```bash
node --version
# v18.x.x hoặc cao hơn

npm --version
# 9.x.x hoặc cao hơn

aws sts get-caller-identity
# Xác nhận đúng account và region

# Lấy API URL từ SAM stack output
aws cloudformation describe-stacks \
  --stack-name hireflow-prod \
  --query 'Stacks[0].Outputs[?OutputKey==`ApiUrl`].OutputValue' \
  --output text
# https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod
```