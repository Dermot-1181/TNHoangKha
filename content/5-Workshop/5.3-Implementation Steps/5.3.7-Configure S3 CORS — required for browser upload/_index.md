---
title : "Configure S3 CORS — required for browser upload"
date : 2026-07-10 
weight : 7
chapter : false
pre : " <b> 5.3.7 </b> "
---




The browser performs a CORS preflight (`OPTIONS`) before every cross-origin request. The S3 Quarantine bucket needs a CORS policy allowing browsers from the Candidate App domain to issue PUT requests:

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

**What happens without CORS:** The browser receives no `Access-Control-Allow-Origin` header in the S3 preflight response → the request is blocked with `CORS policy: No 'Access-Control-Allow-Origin' header`. Even with a valid presigned URL, the upload will fail.

**`x-amz-meta-*` headers must be declared in `AllowedHeaders`:** The browser sends these headers in the PUT request to store metadata in S3. If not declared, S3 rejects the preflight.