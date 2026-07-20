---
title : "Cấu hình S3 CORS — bắt buộc cho browser upload"
date : 2026-07-10 
weight : 7
chapter : false
pre : " <b> 5.3.7 </b> "
---

Browser thực hiện CORS preflight (`OPTIONS`) trước mọi cross-origin request. S3 Quarantine bucket cần CORS policy cho phép browser từ domain Candidate App gọi PUT:

```bash
aws s3api put-bucket-cors \
  --bucket hireflow-quarantine-226593872610 \
  --cors-configuration '{
    "CORSRules": [{
      "AllowedOrigins": [
        "https://<cloudfront-domain>.cloudfront.net",
        "http://localhost:5173"
      ],
      "AllowedMethods": ["PUT", "POST", "GET", "HEAD"],
      "AllowedHeaders": [
        "Content-Type",
        "x-amz-meta-channel",
        "x-amz-meta-job_id",
        "x-amz-server-side-encryption"
      ],
      "ExposedHeaders": ["ETag"],
      "MaxAgeSeconds": 3000
    }]
  }'
```
