---
title : "Drag & Drop và UX Upload"
date : 2026-07-10 
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---

Candidate App hỗ trợ cả click chọn file lẫn kéo thả, với feedback trạng thái rõ ràng.

**Dropzone component (trong App.jsx):**

```jsx
<div
  className={`dropzone ${dragOver ? 'drag-over' : ''} ${file ? 'has-file' : ''}`}
  onClick={() => fileRef.current.click()}
  onDragOver={e => { e.preventDefault(); setDragOver(true) }}
  onDragLeave={() => setDragOver(false)}
  onDrop={e => {
    e.preventDefault()
    setDragOver(false)
    handleFile(e.dataTransfer.files[0])  // lấy file đầu tiên từ drag
  }}
  role="button"
  aria-label="Khu vực tải file CV"
  tabIndex={0}
  onKeyDown={e => e.key === 'Enter' && fileRef.current.click()}
>
  <input
    ref={fileRef}
    type="file"
    accept=".pdf,.docx,.doc"
    style={{ display: 'none' }}
    onChange={e => handleFile(e.target.files[0])}
  />
  {file ? (
    <>
      <span className="file-icon">📄</span>
      <span className="file-name">{file.name}</span>
      <span className="file-size">({(file.size / 1024 / 1024).toFixed(2)} MB)</span>
    </>
  ) : (
    <>
      <span className="upload-icon">📁</span>
      <span className="drop-text">Kéo thả file vào đây hoặc <u>bấm để chọn</u></span>
      <span className="drop-hint">PDF, DOCX, DOC — tối đa 5MB</span>
    </>
  )}
</div>
```

**Accessibility:** Dropzone có `role="button"`, `tabIndex={0}`, và `onKeyDown` để người dùng dùng bàn phím có thể kích hoạt. `aria-label` mô tả chức năng cho screen reader.

**Load danh sách job từ API lúc khởi động:**

```jsx
useEffect(() => {
  let cancelled = false
  async function loadJobs() {
    setJobsLoading(true)
    try {
      const { data } = await axios.get(`${API_BASE}/dashboard/jobs`)
      const list = Array.isArray(data?.jobs) ? data.jobs : []
      if (!cancelled) setJobs(list)
    } catch (err) {
      if (!cancelled) setJobsError('Không tải được danh sách vị trí.')
    } finally {
      if (!cancelled) setJobsLoading(false)
    }
  }
  void loadJobs()
  return () => { cancelled = true }  // cleanup tránh setState sau unmount
}, [hydrated])
```

**`useLayoutEffect` để sync URL params trước paint:**

```jsx
useLayoutEffect(() => {
  // Đọc ?job_id= từ URL để HR có thể gửi link trực tiếp cho ứng viên
  const params = new URLSearchParams(window.location.search)
  const jid = params.get('job_id')
  if (jid) setJobId(jid)
  setHydrated(true)
}, [])
```

`useLayoutEffect` chạy đồng bộ trước khi browser paint, tránh flash giao diện khi state thay đổi từ URL params.