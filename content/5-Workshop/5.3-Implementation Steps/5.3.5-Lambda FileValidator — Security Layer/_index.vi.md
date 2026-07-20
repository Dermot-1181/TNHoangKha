---
title : "Lambda FileValidator — Security Layer"
date : 2026-07-10 
weight : 5
chapter : false
pre : " <b> 5.3.5 </b> "
---

Sau khi file lên S3 Quarantine, EventBridge tự động trigger Lambda FileValidator. Lambda này là lớp bảo mật đầu tiên trước khi file đi vào pipeline xử lý.

**Trigger:** EventBridge rule `hireflow-quarantine-upload` — bắt event `S3 ObjectCreated` trên prefix `web/` của Quarantine bucket (rule được tạo ngoài SAM bằng AWS CLI, xem Workshop 1 Step 2 để biết lý do và cách tạo).

**Luồng kiểm tra:**

```
S3 ObjectCreated event (S3 CV Raw / Quarantine)
    ↓
head_object → lấy ContentType, size, metadata (job_id, channel)
    ↓
get_object → đọc toàn bộ file bytes
    ↓
_detect_mime_by_magic() → kiểm tra magic bytes thực tế của file
    ↓
Kiểm tra kích thước ≤ 5MB
    ↓
SHA-256 hash → query DynamoDB GSI sha256_hash-index (dedup)
    ↓
PASS: put_object sang S3 CV Files (Clean) + delete_object ở Quarantine
FAIL: SNS alert HR, giữ file Quarantine (lifecycle 7 ngày tự xóa)
```

**Kiểm tra MIME bằng magic bytes — chống giả extension:**

```python
# src/file_validator/app.py

MAGIC_BYTES = {
    b'%PDF':          'application/pdf',
    b'PK\x03\x04':   'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
    b'\xd0\xcf\x11\xe0': 'application/msword',  # Compound Document (DOC cũ)
}

def _detect_mime_by_magic(file_bytes: bytes) -> str | None:
    """
    Kiểm tra magic bytes đầu file thay vì tin vào Content-Type header.
    PDF từ macOS Preview / Word đôi khi có BOM hoặc whitespace trước '%PDF'
    → lstrip trước khi so sánh.
    """
    stripped = file_bytes.lstrip(b' \t\r\n\x00\x0b\x0c\xef\xbb\xbf')
    for magic, mime in MAGIC_BYTES.items():
        if stripped[:len(magic)] == magic:
            return mime
    return None
```

**Điểm quan trọng:** `Content-Type` header trong S3 object metadata có thể bị spoof — ví dụ file `.exe` đổi tên thành `cv.pdf` sẽ vẫn có `Content-Type: application/pdf` nhưng magic bytes sẽ lộ rõ đây không phải PDF thật. Magic bytes `%PDF` là 4 byte đầu của mọi file PDF hợp lệ theo chuẩn ISO 32000.

**Deduplication bằng SHA-256:**

```python
# src/file_validator/app.py

def _is_duplicate(sha256_hash: str) -> bool:
    """
    Query DynamoDB GSI 'sha256_hash-index' để phát hiện file đã nộp trước đó.
    Fail-open: nếu DynamoDB lỗi tạm thời → cho qua để không block ứng viên.
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

**Copy sang Clean + xóa Quarantine:**

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
        # Không fail cả flow — S3 lifecycle 7 ngày sẽ dọn dẹp
        print(f'[WARN] Không xóa được Quarantine: {str(e)}')
```

**Metadata được giữ nguyên khi copy** sang S3 CV Files để Lambda Extract (bước 8 trong diagram) có thể đọc `job_id`, `channel`, `sha256_hash` mà không cần parse lại từ object key.