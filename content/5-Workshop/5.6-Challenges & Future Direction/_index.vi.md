---
title : "Challenges & Future Direction"
date : 2026-07-10
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---


### Thách thức đã gặp và cách giải quyết

**Signature Mismatch 403 khi upload lên S3**

Vấn đề phổ biến nhất khi dùng presigned URL. Lambda sign URL với `ContentType: application/pdf`, nhưng axios khi nhận `File` object tự thêm `Content-Type: multipart/form-data` vào request → S3 từ chối với HTTP 403.

Giải pháp: Convert `File` sang `ArrayBuffer` trước khi PUT. `ArrayBuffer` là raw binary, axios không tự thêm Content-Type. Kết hợp với việc luôn map Content-Type từ extension thay vì dùng `file.type` (browser đôi khi trả về chuỗi rỗng cho PDF trên macOS Safari).

**CORS preflight fail trên S3**

S3 CORS policy cần khai báo chính xác các `AllowedHeaders` gồm cả `x-amz-meta-*` headers. Thiếu bất kỳ header nào → preflight OPTIONS trả 403 → browser block request. Debug bằng cách mở Network tab F12, xem response của OPTIONS request.

**Browser cache giao diện cũ sau khi deploy**

Sau mỗi lần build mới deploy lên S3, CloudFront vẫn cache bản cũ. Giải pháp: Vite tự động thêm content hash vào tên file JS/CSS (`index-Bx3kP9aZ.js`) → cache vĩnh viễn an toàn. Riêng `index.html` set `no-cache`. Sau deploy chạy CloudFront invalidation `/*` để flush cache ngay.

**DOM shift và `removeChild` error khi React re-render**

Khi trạng thái chuyển từ `uploading` → `success`, React unmount form và mount success state. Nếu browser extension (như LastPass, Grammarly) đã inject DOM node vào form → React không tìm thấy node khi cleanup → lỗi `removeChild`. Giải pháp: thêm `key={status}` vào wrapper div để ép React khởi tạo lại cây con hoàn toàn khi status thay đổi, không tái sử dụng DOM node cũ.

**EventBridge rule phải tạo ngoài SAM**

Circular dependency giữa S3 Quarantine bucket và Lambda FileValidator Permission đã được giải quyết ở mức SAM template — xem Workshop 1 để biết chi tiết kỹ thuật. Tác động với luồng upload: EventBridge rule `hireflow-quarantine-upload` phải được tạo riêng bằng AWS CLI sau khi stack deploy, không thể tự động hóa trong cùng `sam deploy`.

### Hướng phát triển tương lai

**Virus scan tự động với Amazon GuardDuty Malware Protection**

Tích hợp GuardDuty để scan file ngay khi vào Quarantine. Code FileValidator đã có sẵn handler cho GuardDuty event format (`THREATS_FOUND` → xóa file ngay + alert HR). Chỉ cần bật GuardDuty Malware Protection cho S3 bucket là active.

**Upload nhiều file cùng lúc**

Hiện tại form chỉ nhận 1 file. Mở rộng bằng cách cho phép chọn nhiều file, tạo nhiều presigned URL song song (`Promise.all`), upload đồng thời với progress tracking riêng từng file.

**Track trạng thái xử lý sau upload**

Sau khi upload thành công, ứng viên không biết CV đang ở bước nào. Có thể thêm polling endpoint `GET /apply/status?candidate_id=...` để hiển thị tiến trình: `uploaded → validating → scoring → done`.

**Presigned URL cho mobile app**

Cùng Lambda `GetPresignedUrl` có thể phục vụ mobile app (iOS/Android) mà không cần thay đổi backend — chỉ cần mobile app gọi API lấy URL và PUT file lên S3. Kiến trúc presigned URL tự nhiên hỗ trợ multi-platform.
