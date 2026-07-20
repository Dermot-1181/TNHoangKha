---
title : "Drag & Drop và UX Upload"
date : 2026-07-10 
weight : 4
chapter : false
pre : " <b> 5.3.4 </b> "
---


Candidate App supports both click-to-select and drag-and-drop, with clear state feedback throughout.

**Dropzone component (in App.jsx):**

```jsx
<div
  className={`dropzone ${dragOver ? 'drag-over' : ''} ${file ? 'has-file' : ''}`}
  onClick={() => fileRef.current.click()}
  onDragOver={e => { e.preventDefault(); setDragOver(true) }}
  onDragLeave={() => setDragOver(false)}
  onDrop={e => {
    e.preventDefault()
    setDragOver(false)
    handleFile(e.dataTransfer.files[0])
  }}
  role="button"
  aria-label="CV file upload area"
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
      <span className="drop-text">Drag and drop a file here or <u>click to browse</u></span>
      <span className="drop-hint">PDF, DOCX, DOC — max 5MB</span>
    </>
  )}
</div>
```

**Accessibility:** The dropzone has `role="button"`, `tabIndex={0}`, and `onKeyDown` so keyboard users can activate it. `aria-label` describes the function to screen readers.

**Load job list from API on startup:**

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
      if (!cancelled) setJobsError('Could not load job list.')
    } finally {
      if (!cancelled) setJobsLoading(false)
    }
  }
  void loadJobs()
  return () => { cancelled = true }  // cleanup: avoid setState after unmount
}, [hydrated])
```

**`useLayoutEffect` to sync URL params before paint:**

```jsx
useLayoutEffect(() => {
  // Read ?job_id= from URL so HR can send a direct link to candidates
  const params = new URLSearchParams(window.location.search)
  const jid = params.get('job_id')
  if (jid) setJobId(jid)
  setHydrated(true)
}, [])
```

`useLayoutEffect` runs synchronously before the browser paints, preventing UI flash when state changes from URL params.