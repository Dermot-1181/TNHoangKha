---
title : "Create a gateway endpoint"
date : 2026-07-10 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---


```bash
# Create new project
npm create vite@latest candidate-app -- --template react
cd candidate-app
npm install

# Install axios for API calls
npm install axios
```

Directory structure:

```
candidate-app/
├── index.html
├── vite.config.js
├── package.json
├── .env                    # VITE_API_BASE=<API_GATEWAY_URL>/prod
├── .env.production         # for production build
└── src/
    ├── main.jsx
    ├── App.jsx             # Main component — all upload logic
    ├── config.js           # API base URL
    ├── App.css
    └── index.css
```

Create `src/config.js` — reads API URL from environment variable:

```js
// src/config.js
const API_BASE = import.meta.env.VITE_API_BASE
  || 'https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod';

export { API_BASE };
export default API_BASE;
```

Create `.env` for local development:

```bash
VITE_API_BASE=https://jqsm674y2b.execute-api.ap-southeast-2.amazonaws.com/prod
```

**Why use environment variables:** The API Gateway URL changes with every new stack deployment. Using `VITE_API_BASE` allows building once and deploying to multiple environments without touching source code.