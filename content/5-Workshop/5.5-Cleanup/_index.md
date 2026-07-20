---
title : "Cleanup"
date : 2026-07-10
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---


### Delete frontend S3 bucket

```bash
# Empty bucket first
aws s3 rm s3://hireflow-candidate-226593872610/ --recursive

# Delete bucket
aws s3api delete-bucket --bucket hireflow-candidate-226593872610
```

### Delete CloudFront distribution

```bash
# Disable distribution first (must wait for status Deployed)
aws cloudfront update-distribution \
  --id <DISTRIBUTION_ID> \
  --if-match $(aws cloudfront get-distribution-config --id <DISTRIBUTION_ID> \
               --query ETag --output text) \
  --distribution-config '{"Enabled": false, ...}'

# Once status = Deployed, delete the distribution
aws cloudfront delete-distribution \
  --id <DISTRIBUTION_ID> \
  --if-match <ETAG>
```

### Delete Quarantine bucket content and Lambda logs

Note: The S3 Quarantine bucket and Lambda functions are created inside the SAM stack — they will be removed together when running `sam delete`. The bucket just needs to be emptied first because SAM reports an error if buckets still contain objects:

```bash
# Empty Quarantine bucket before deleting stack
aws s3 rm s3://hireflow-quarantine-<ACCOUNT_ID>/ --recursive

# Delete Lambda log groups
aws logs delete-log-group --log-group-name /aws/lambda/hireflow-get-presigned-url
aws logs delete-log-group --log-group-name /aws/lambda/hireflow-file-validator

# Delete EventBridge rule (created outside SAM — must be removed separately)
aws events remove-targets \
  --rule hireflow-quarantine-upload \
  --ids FileValidatorTarget

aws events delete-rule --name hireflow-quarantine-upload
```

### Delete SAM stack (Lambda, API Gateway, S3, DynamoDB)

```bash
sam delete --stack-name hireflow
```

SAM deletes all resources defined in `template.yaml`. See Workshop 1 for the full cleanup procedure including Clean bucket and DynamoDB tables.
