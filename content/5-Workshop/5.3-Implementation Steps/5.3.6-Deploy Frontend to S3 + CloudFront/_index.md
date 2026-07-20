---
title : "Deploy Frontend to S3 + CloudFront"
date : 2026-07-10 
weight : 6
chapter : false
pre : " <b> 5.3.6 </b> "
---



```bash
cd candidate-app

# Build production bundle
npm run build
# → creates dist/ with minified index.html, JS, CSS

# Sync to frontend S3 bucket
aws s3 sync dist/ s3://hireflow-candidate-226593872610/ \
  --delete \
  --cache-control "max-age=31536000,immutable"   # hashed JS/CSS → cache 1 year

# index.html must not be cached so new deploys take effect immediately
aws s3 cp dist/index.html s3://hireflow-candidate-226593872610/index.html \
  --cache-control "no-cache,no-store,must-revalidate"

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id <DISTRIBUTION_ID> \
  --paths "/*"
```

**Why differentiate cache-control between `index.html` and other assets:**
Vite build appends a content hash to each JS/CSS filename (e.g. `index-Bx3kP9aZ.js`) — the hash changes whenever code changes, so permanent caching is safe. `index.html` has no hash, so it must be `no-cache` to ensure users always receive the latest version containing the correct asset filenames.

**Configure S3 bucket for static hosting:**

```bash
# Enable static website hosting
aws s3 website s3://hireflow-candidate-226593872610/ \
  --index-document index.html \
  --error-document index.html   # SPA routing: 404 falls back to index.html

# Set public read bucket policy (or use CloudFront OAC)
aws s3api put-bucket-policy \
  --bucket hireflow-candidate-226593872610 \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [{
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::hireflow-candidate-226593872610/*"
    }]
  }'
```
