---
title : "Lambda FileValidator — Security Layer"
date : 2026-07-10 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---



Once a file lands in S3 Quarantine, EventBridge automatically triggers Lambda FileValidator. This Lambda is the first security layer before any file enters the processing pipeline.

**Trigger:** EventBridge rule `hireflow-quarantine-upload` — catches `S3 ObjectCreated` events on prefix `web/` of the Quarantine bucket (rule created outside SAM via AWS CLI — see Workshop 1 Step 2 for the reason and creation commands).

**Validation flow:**

```
S3 ObjectCreated event (S3 CV Raw / Quarantine)
    ↓
head_object → get ContentType, size, metadata (job_id, channel)
    ↓
get_object → read full file bytes
    ↓
_detect_mime_by_magic() → verify actual file type via magic bytes
    ↓
Check file size ≤ 5MB
    ↓
SHA-256 hash → query DynamoDB GSI sha256_hash-index (dedup)
    ↓
PASS: put_object to S3 CV Files (Clean) + delete_object from Quarantine
FAIL: SNS alert HR, file stays in Quarantine (7-day lifecycle auto-cleanup)
```

**MIME validation via magic bytes — prevent extension spoofing:**

```python
# src/file_validator/app.py

MAGIC_BYTES = {
    b'%PDF':             'application/pdf',
    b'PK\x03\x04':      'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
    b'\xd0\xcf\x11\xe0': 'application/msword',  # Compound Document (legacy DOC)
}

def _detect_mime_by_magic(file_bytes: bytes) -> str | None:
    """
    Check magic bytes at the start of the file instead of trusting Content-Type header.
    PDFs exported by macOS Preview or Word sometimes have a BOM or whitespace
    before the '%PDF' marker — lstrip before comparing.
    """
    stripped = file_bytes.lstrip(b' \t\r\n\x00\x0b\x0c\xef\xbb\xbf')
    for magic, mime in MAGIC_BYTES.items():
        if stripped[:len(magic)] == magic:
            return mime
    return None
```

**Important:** The `Content-Type` header in S3 object metadata can be spoofed — for example, an `.exe` renamed to `cv.pdf` will still have `Content-Type: application/pdf` but its magic bytes will reveal it is not a real PDF. The magic bytes `%PDF` are the first 4 bytes of every valid PDF per the ISO 32000 standard.

**SHA-256 deduplication:**

```python
# src/file_validator/app.py

def _is_duplicate(sha256_hash: str) -> bool:
    """
    Query DynamoDB GSI 'sha256_hash-index' to detect previously submitted files.
    Fail-open: if DynamoDB has a transient error, allow the file through
    rather than blocking the candidate.
    """
    table = dynamodb.Table(CANDIDATES_TABLE)
    try:
        response = table.query(
            IndexName='sha256_hash-index',
            KeyConditionExpression='sha256_hash = :h',
            ExpressionAttributeValues={':h': sha256_hash},
            Limit=1
        )
        return len(response.get('Items', [])) > 0
    except ClientError as e:
        print(f'[ERROR] DynamoDB GSI query failed: {str(e)}')
        return False  # fail-open
```

**Copy to Clean + delete from Quarantine:**

```python
# src/file_validator/app.py

def _copy_to_clean(src_bucket, src_key, file_bytes, content_type,
                   job_id, channel, sha256_hash):
    s3.put_object(
        Bucket=CLEAN_BUCKET,
        Key=src_key,
        Body=file_bytes,
        ContentType=content_type,
        Metadata={
            'job_id':      job_id,
            'channel':     channel,
            'sha256_hash': sha256_hash,
            'source_key':  src_key,
        }
    )
    try:
        s3.delete_object(Bucket=src_bucket, Key=src_key)
    except ClientError as e:
        # Don't fail the whole flow — S3 lifecycle will clean up in 7 days
        print(f'[WARN] Could not delete from Quarantine: {str(e)}')
```

**Metadata is preserved on copy** to S3 CV Files so that the downstream Lambda Extract (step 8 in the diagram) can read `job_id`, `channel`, and `sha256_hash` directly from the object metadata without re-parsing the S3 key.