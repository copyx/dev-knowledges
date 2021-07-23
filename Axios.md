# Axios

## 주의사항

### 요청 URL의 상대경로와 절대경로의 차이

상대경로는 baseURL을 기준으로 요청 URL이 결정. 절대경로는 baseURL을 무시하고 입력한 URL만 사용.

```javascript
import axios from "axios";

const api = axios.create({
  baseURL: "https://api.themoviedb.org/3",
  params: {
    api_key: "ac3cac094225c9ea0022a8d0705ae88c",
    language: "en-US",
  },
});

api.get("tv/popular"); // https://api.themoviedb.org/3/tv/popular
api.get("/tv/popular"); // /tv/popular
```
