---
title : "Deploy Frontend lên S3 + CloudFront"
date : 2026-07-10 
weight : 6
chapter : false
pre : " <b> 5.3.6 </b> "
---


```bash
cd candidate-app

# Build production bundle
npm run build
# → tạo thư mục dist/ với index.html, JS, CSS đã minify

# Upload lên S3 bucket frontend
aws s3 sync dist/ s3://hireflow-candidate-226593872610/ \
  --delete \
  --cache-control "max-age=31536000,immutable"   # JS/CSS hash → cache 1 năm

# index.html không cache để deploy mới có hiệu lực ngay
aws s3 cp dist/index.html s3://hireflow-candidate-226593872610/index.html \
  --cache-control "no-cache,no-store,must-revalidate"

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id <DISTRIBUTION_ID> \
  --paths "/*"
```

**Tại sao phân biệt cache-control giữa `index.html` và assets:**
Vite build tạo ra tên file JS/CSS có hash nội dung (ví dụ `index-Bx3kP9aZ.js`) — hash thay đổi khi code đổi nên cache vĩnh viễn an toàn. `index.html` không có hash, phải `no-cache` để người dùng luôn nhận bản mới nhất chứa đúng tên file JS.

**Cấu hình S3 bucket cho static hosting:**

```bash
# Bật static website hosting
aws s3 website s3://hireflow-candidate-226593872610/ \
  --index-document index.html \
  --error-document index.html   # SPA routing: 404 về index.html

# Set bucket policy public read (hoặc dùng CloudFront OAC)
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