---
title : "Cleanup"
date : 2026-07-10
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---


### Xóa frontend S3 bucket

```bash
# Empty bucket trước
aws s3 rm s3://hireflow-candidate-226593872610/ --recursive

# Xóa bucket
aws s3api delete-bucket --bucket hireflow-candidate-226593872610
```

### Xóa CloudFront distribution

```bash
# Disable distribution trước (phải đợi deployed)
aws cloudfront update-distribution \
  --id <DISTRIBUTION_ID> \
  --if-match $(aws cloudfront get-distribution-config --id <DISTRIBUTION_ID> \
               --query ETag --output text) \
  --distribution-config '{"Enabled": false, ...}'

# Sau khi status = Deployed, xóa distribution
aws cloudfront delete-distribution \
  --id <DISTRIBUTION_ID> \
  --if-match <ETAG>
```

### Xóa Quarantine bucket và Lambda logs

Lưu ý: S3 Quarantine bucket và Lambda được tạo trong SAM stack — chúng sẽ bị xóa cùng khi chạy `sam delete`. Chỉ cần empty bucket trước vì SAM báo lỗi nếu bucket còn object:

```bash
# Empty Quarantine bucket trước khi xóa stack
aws s3 rm s3://hireflow-quarantine-226593872610/ --recursive

# Xóa Lambda log groups
aws logs delete-log-group --log-group-name /aws/lambda/hireflow-get-presigned-url
aws logs delete-log-group --log-group-name /aws/lambda/hireflow-file-validator

# Xóa EventBridge rule (tạo ngoài SAM — phải xóa riêng)
aws events remove-targets \
  --rule hireflow-quarantine-upload \
  --ids FileValidatorTarget

aws events delete-rule --name hireflow-quarantine-upload
```

### Xóa SAM stack (toàn bộ Lambda + API Gateway + S3 + DynamoDB)

```bash
cd hireflow-sam
sam delete --stack-name hireflow-prod
```
