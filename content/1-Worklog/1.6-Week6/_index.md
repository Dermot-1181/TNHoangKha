---

## title: "Week 6 Worklog"

date: 2026-05-25
weight: 1
chapter: false
pre: " **1.6.** "

### Week 6 Objectives:

- Complete the end-to-end CV upload workflow: `GetPresignedUrl` Lambda + Candidate App + `FileValidator`.
- Correctly handle security edge cases: spoofed file extensions and duplicate files.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                                                                                                                              | Start Date | Completion Date | Reference Material |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 2   | Implement the `get_presigned_url` Lambda function: validate `job_id` to block path traversal, validate extensions, and generate a SigV4 presigned PUT URL with an expiration of 900 seconds. Deploy via SAM and test using `curl` successfully.                                                                                   | 25/05/2026 | 25/05/2026      |                    |
| 3   | Set up the Candidate App (React + Vite): run `npm create vite@latest`, install `axios`, and configure `.env` with `VITE_API_BASE`. Implement the upload form with client-side validation for file types (PDF/DOCX/DOC) and file size ($\le$ 5MB).                                                                                 | 26/05/2026 | 26/05/2026      |                    |
| 4   | Implement the 2-step upload mechanism in `App.jsx`: GET presigned URL $\rightarrow$ PUT file to S3. Debug a 403 Signature Mismatch error caused by `axios` automatically appending `multipart/form-data`. Fix: convert the `File` object to an `ArrayBuffer` and map the `Content-Type` precisely from the file extension.        | 27/05/2026 | 27/05/2026      |                    |
| 5   | Learn EC2 fundamentals:• Instance types, AMIs, EBS, etc.• Methods for remote SSH access into EC2.• Research Elastic IPs.                                                                                                                                                                                                       | 28/05/2026 | 28/05/2026      |                    |
| 6   | Finalize `FileValidator`: copy files to the Clean S3 bucket while preserving metadata, delete them from the Quarantine bucket, and trigger an SNS alert to HR on validation failure. Implement drag-and-drop, a real-time progress bar, and handle success/error states utilizing `key={status}`. Test all validation edge cases. | 29/05/2026 | 29/05/2026      |                    |




### Week 6 Achievements:

- Successfully uploaded CVs directly from the browser to the S3 Quarantine bucket via presigned URLs.
- `FileValidator` accurately detects spoofed file extensions by inspecting magic bytes and blocks duplicate submissions using SHA-256 hashing.
- Completed the Candidate App UX: fully integrated drag-and-drop capability, real-time progress tracking, and robust error handling.

