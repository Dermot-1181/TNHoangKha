---
title : "Lambda GetPresignedUrl — generate S3 upload URL"
date : 2026-07-10 
weight : 2
chapter : false
pre : " <b>5.3.2</b> "

---

This Lambda is the entry point of the entire upload flow. It never receives the file — it only generates a signed URL for the browser to use when uploading directly to S3.

**Route:** `GET /apply/presigned-url?job_id=...&file_name=...`

**Processing flow:**

```
Browser sends GET /apply/presigned-url
    ↓
Validate job_id (alphanumeric, prevent path traversal)
    ↓
Validate extension (.pdf / .docx / .doc)
    ↓
Generate key: web/<job_id>/<uuid>.<ext>
    ↓
s3.generate_presigned_url with ContentType + Metadata
    ↓
Return { presigned_url, file_key, expires_in_seconds: 900 }
```

**Lambda code —** `src/get_presigned_url/app.py`**:**

```python
import json, boto3, uuid, os
from botocore.config import Config

REGION = os.environ.get('AWS_REGION', 'ap-southeast-2')
s3 = boto3.client(
    's3',
    region_name=REGION,
    endpoint_url=f'https://s3.{REGION}.amazonaws.com',
    config=Config(signature_version='s3v4'),   # SigV4 required for presigned PUT
)
QUARANTINE_BUCKET = os.environ['QUARANTINE_BUCKET']
URL_EXPIRATION_SECONDS = 900  # 15 minutes
ALLOWED_EXTENSIONS = ('.pdf', '.docx', '.doc')

def handler(event, context):
    params = event.get('queryStringParameters') or {}
    job_id    = params.get('job_id', '').strip()
    file_name = params.get('file_name', '').strip()

    if not job_id:
        return _response(400, {'error': 'Missing job_id'})
    if not file_name:
        return _response(400, {'error': 'Missing file_name'})

    # Block path traversal: job_id must be alphanumeric, - and _ only
    if not job_id.replace('-', '').replace('_', '').isalnum():
        return _response(400, {'error': 'Invalid job_id'})

    ext = os.path.splitext(file_name)[1].lower()
    if ext not in ALLOWED_EXTENSIONS:
        return _response(400, {'error': 'Only PDF, DOCX, DOC accepted'})

    file_id  = str(uuid.uuid4())
    file_key = f'web/{job_id}/{file_id}{ext}'

    presigned_url = s3.generate_presigned_url(
        ClientMethod='put_object',
        Params={
            'Bucket': QUARANTINE_BUCKET,
            'Key':    file_key,
            'ContentType': _content_type_for(ext),
            'Metadata': {
                'job_id':  job_id,
                'channel': 'web'
            }
        },
        ExpiresIn=URL_EXPIRATION_SECONDS
    )

    return _response(200, {
        'presigned_url': presigned_url,
        'file_key': file_key,
        'expires_in_seconds': URL_EXPIRATION_SECONDS
    })
```

**Important — why** `signature_version='s3v4'` **is required:**
S3 Presigned URLs use SigV4 (Signature Version 4). Without explicit config, the SDK may fall back to SigV2 (deprecated) → URL rejected with 403. Explicit config ensures SigV4 is always used with the correct region.

**Important — why** `ContentType` **is included in the presigned URL signature:**
When the browser PUTs a file to S3, S3 verifies that the `Content-Type` header matches the signed value. If the browser sends `Content-Type: multipart/form-data` (default for HTML `<form>`) instead of `application/pdf` → S3 returns 403 Signature Mismatch. The frontend must therefore set the exact same Content-Type when performing the PUT.