---
title : "Tạo một Gateway Endpoint"
date : 2026-07-10 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

```bash
# Tạo project mới
npm create vite@latest candidate-app -- --template react
cd candidate-app
npm install

# Cài axios để gọi API
npm install axios
```

Cấu trúc thư mục:

```
candidate-app/
├── index.html
├── vite.config.js
├── package.json
├── .env                    # VITE_API_BASE=<API_GATEWAY_URL>/prod
├── .env.production         # cho production build
└── src/
    ├── main.jsx
    ├── App.jsx             # Component chính — toàn bộ upload logic
    ├── config.js           # API base URL
    ├── App.css
    └── index.css
```

Tạo file `src/config.js` — đọc API URL từ environment variable:

```js
// src/config.js
const API_BASE = import.meta.env.VITE_API_BASE
  || 'https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod';

export { API_BASE };
export default API_BASE;
```

Tạo file `.env` cho development local:

```bash
VITE_API_BASE=https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod
```

**Tại sao dùng environment variable:** URL API Gateway thay đổi mỗi lần deploy stack mới. Dùng `VITE_API_BASE` cho phép build một lần, deploy nhiều môi trường mà không cần sửa code.
