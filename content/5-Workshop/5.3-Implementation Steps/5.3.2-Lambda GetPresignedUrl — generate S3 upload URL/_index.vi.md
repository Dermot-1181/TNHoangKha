---
title : "Lambda GetPresignedUrl — generate S3 upload URL"
date : 2026-07-10 
weight : 2
chapter : false
pre : " <b>5.3.2</b> "
---


Lambda này là điểm khởi đầu của toàn bộ luồng upload. Nó không nhận file — chỉ tạo một URL có chữ ký để browser dùng upload thẳng lên S3.

**Route:** `GET /apply/presigned-url?job_id=...&file_name=...`

**Luồng xử lý:**

```
Browser gửi GET /apply/presigned-url
    ↓
Validate job_id (alphanum, không path traversal)
    ↓
Validate extension (.pdf / .docx / .doc)
    ↓
Tạo key: web/<job_id>/<uuid>.<ext>
    ↓
s3.generate_presigned_url với ContentType + Metadata
    ↓
Trả về { presigned_url, file_key, expires_in_seconds: 900 }
```

**Code Lambda —** `src/get_presigned_url/app.py`**:**

```python
import json, boto3, uuid, os
from botocore.config import Config

REGION = os.environ.get('AWS_REGION', 'ap-southeast-2')
s3 = boto3.client(
    's3',
    region_name=REGION,
    endpoint_url=f'https://s3.{REGION}.amazonaws.com',
    config=Config(signature_version='s3v4'),   # SigV4 bắt buộc cho presigned PUT
)
QUARANTINE_BUCKET = os.environ['QUARANTINE_BUCKET']
URL_EXPIRATION_SECONDS = 900  # 15 phút
ALLOWED_EXTENSIONS = ('.pdf', '.docx', '.doc')

def handler(event, context):
    params = event.get('queryStringParameters') or {}
    job_id   = params.get('job_id', '').strip()
    file_name = params.get('file_name', '').strip()

    if not job_id:
        return _response(400, {'error': 'Thiếu job_id'})
    if not file_name:
        return _response(400, {'error': 'Thiếu file_name'})

    # Chặn path traversal: job_id chỉ chứa ký tự alphanum, - và _
    if not job_id.replace('-', '').replace('_', '').isalnum():
        return _response(400, {'error': 'job_id không hợp lệ'})

    ext = os.path.splitext(file_name)[1].lower()
    if ext not in ALLOWED_EXTENSIONS:
        return _response(400, {'error': 'Chỉ nhận PDF, DOCX, DOC'})

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

**Điểm quan trọng — tại sao cần** `signature_version='s3v4'`**:**
S3 Presigned URL dùng SigV4 (Signature Version 4). Nếu không chỉ định, SDK có thể dùng SigV2 (đã deprecated) → URL bị từ chối với lỗi 403. Explicit config đảm bảo luôn dùng SigV4 đúng region.

**Điểm quan trọng — tại sao sign cả** `ContentType` **trong presigned URL:**
Khi browser PUT file lên S3, S3 sẽ verify `Content-Type` header phải khớp với giá trị đã sign. Nếu browser gửi `Content-Type: multipart/form-data` (mặc định của `<form>`) thay vì `application/pdf` → S3 trả 403 Signature Mismatch. Vì vậy frontend cũng phải set đúng Content-Type khi PUT.