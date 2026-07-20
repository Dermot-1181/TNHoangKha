---
title : "Kiểm tra Gateway Endpoint"
date : 2026-07-10 
weight : 3
chapter : false
pre : " <b> 5.3.3 </b> "
---

Toàn bộ logic upload nằm trong `App.jsx`. Quy trình gồm 2 bước rõ ràng:

**Bước 1:** Gọi API lấy presigned URL  
**Bước 2:** PUT file thẳng lên S3 với URL đó

**Validate file trước khi gọi API:**

```jsx
// src/App.jsx
const ALLOWED_TYPES = [
  'application/pdf',
  'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  'application/msword'
]
const MAX_SIZE_MB = 5

function handleFile(f) {
  if (!f) return
  if (!ALLOWED_TYPES.includes(f.type)) {
    setMessage('Chỉ chấp nhận file PDF, DOCX hoặc DOC.')
    return
  }
  if (f.size > MAX_SIZE_MB * 1024 * 1024) {
    setMessage(`File quá lớn. Tối đa ${MAX_SIZE_MB}MB.`)
    return
  }
  setFile(f)
  setMessage('')
}
```

**Vấn đề thực tế — browser báo sai Content-Type:**
Trên một số trình duyệt (đặc biệt macOS Safari), `file.type` của file `.pdf` có thể trả về chuỗi rỗng `""`. Nếu dùng trực tiếp `file.type` để set `Content-Type` khi PUT lên S3 → Signature mismatch vì Lambda đã sign với `application/pdf`. Giải pháp: map từ extension file:

```jsx
// src/App.jsx
const EXT_TO_TYPE = {
  '.pdf':  'application/pdf',
  '.docx': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  '.doc':  'application/msword',
}

function _contentTypeFor(file) {
  const name = (file?.name || '').toLowerCase()
  const dot  = name.lastIndexOf('.')
  if (dot >= 0) {
    const t = EXT_TO_TYPE[name.slice(dot)]
    if (t) return t
  }
  return file?.type || 'application/octet-stream'
}
```

**Submit handler — 2 bước upload:**

```jsx
// src/App.jsx
async function handleSubmit(e) {
  e.preventDefault()
  if (!jobId.trim()) { setMessage('Vui lòng chọn vị trí.'); return }
  if (!file) { setMessage('Vui lòng chọn file CV.'); return }

  setStatus('uploading')

  // Bước 1: Lấy presigned URL từ API Gateway → Lambda
  let data
  try {
    const resp = await axios.get(`${API_BASE}/apply/presigned-url`, {
      params: { job_id: jobId.trim(), file_name: file.name }
    })
    data = resp.data
  } catch (err) {
    const apiMsg = err.response?.data?.error || err.message
    setStatus('error')
    setMessage(`Bước 1 (lấy URL) thất bại: ${apiMsg}`)
    return
  }

  // Bước 2: Upload thẳng lên S3 (KHÔNG qua Lambda)
  try {
    const contentType  = _contentTypeFor(file)
    const arrayBuffer  = await file.arrayBuffer()  // đọc file thành binary

    await axios.put(data.presigned_url, arrayBuffer, {
      headers: {
        'Content-Type': contentType,              // phải khớp với URL đã sign
        'x-amz-meta-channel': 'web',
        'x-amz-meta-job_id':  jobId.trim()
      },
      onUploadProgress: (e) => {
        if (e.total > 0) {
          const pct = Math.round((e.loaded / e.total) * 100)
          setMessage(`Đang tải lên... ${pct}%`)
        }
      }
    })
  } catch (err) {
    setStatus('error')
    const status = err.response?.status
    const friendly = status === 403
      ? 'URL hết hạn hoặc sai chữ ký. Thử lại.'
      : status === 400
        ? 'S3 từ chối file (Content-Type có thể sai).'
        : 'Upload lên S3 thất bại.'
    setMessage(`${friendly} (HTTP ${status || '?'})`)
    return
  }

  setStatus('success')
  setFile(null)
}
```

**Tại sao dùng `file.arrayBuffer()` thay vì gửi `file` trực tiếp:**
Axios khi nhận `File` object có thể tự thêm `Content-Type: multipart/form-data` vào request. S3 presigned URL đã sign với `application/pdf` → mismatch → 403. Convert sang `ArrayBuffer` trước khi PUT đảm bảo axios không override Content-Type header.