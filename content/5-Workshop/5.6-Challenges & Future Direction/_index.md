---
title : "Challenges & Future Direction"
date : 2026-07-10
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---


### Challenges encountered and solutions

**Signature Mismatch 403 on S3 upload**

The most common issue when using presigned URLs. Lambda signs the URL with `ContentType: application/pdf`, but axios — when given a `File` object — automatically appends `Content-Type: multipart/form-data` to the request → S3 rejects it with HTTP 403.

Solution: convert `File` to `ArrayBuffer` before the PUT. `ArrayBuffer` is raw binary — axios does not override the Content-Type. Combined with always mapping Content-Type from the file extension rather than relying on `file.type` (some browsers return an empty string for PDFs on macOS Safari).

**CORS preflight failure on S3**

S3 CORS policy must explicitly declare all `AllowedHeaders`, including the `x-amz-meta-*` headers. Any missing header causes the OPTIONS preflight to return 403 → browser blocks the request. Debug by opening the Network tab in DevTools and inspecting the OPTIONS response.

**Stale UI after frontend deploy**

After a new build is deployed to S3, CloudFront still serves the cached version. Solution: Vite automatically appends a content hash to JS/CSS filenames (`index-Bx3kP9aZ.js`) → permanent caching is safe for these assets. `index.html` is set to `no-cache`. After each deploy, run a CloudFront invalidation on `/*` to flush the cache immediately.

**DOM shift and `removeChild` error on React re-render**

When state transitions from `uploading` → `success`, React unmounts the form and mounts the success state. If a browser extension (LastPass, Grammarly, etc.) has injected DOM nodes into the form, React cannot find them during cleanup → `removeChild` error. Solution: add `key={status}` to the wrapper div to force React to fully re-initialize the subtree on each status change, rather than reusing stale DOM nodes.

**EventBridge rule must be created outside SAM**

The circular dependency between the S3 Quarantine bucket and the Lambda FileValidator Permission was resolved at the SAM template level — see Workshop 1 for the technical details. The practical impact on this workshop: the EventBridge rule `hireflow-quarantine-upload` must be created separately via AWS CLI after the stack deploys and cannot be automated within a single `sam deploy`.

### Future directions

**Automatic virus scanning with Amazon GuardDuty Malware Protection**

Integrate GuardDuty to scan files as soon as they land in Quarantine. The FileValidator code already includes a handler for the GuardDuty event format (`THREATS_FOUND` → delete file immediately + alert HR). Enabling GuardDuty Malware Protection for the S3 bucket is sufficient to activate this path.

**Multi-file upload**

The current form accepts a single file. This can be extended to allow selecting multiple files, generating multiple presigned URLs in parallel with `Promise.all`, and uploading them concurrently with per-file progress tracking.

**Upload status tracking**

After a successful upload, the candidate has no visibility into where their CV is in the pipeline. A polling endpoint `GET /apply/status?candidate_id=...` could be added to display real-time progress: `uploaded → validating → scoring → done`.

**Presigned URL for mobile apps**

The same `GetPresignedUrl` Lambda can serve iOS and Android apps without any backend changes — the mobile app simply calls the API to get a URL and PUTs the file to S3. The presigned URL architecture naturally supports multiple client platforms.
