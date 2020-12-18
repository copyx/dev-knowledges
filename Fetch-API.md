# Fetch API

리소스를 가져오기 위한 API(네트워크 통신 포함). 이와 비슷한 XMLHttpRequest가 원래 있었음.

## History

1. XMLHttpRequest API
1. JQuery AJAX
1. Fetch API(ES2015)

> 🤔 XMLHttpRequest, JQuery AJAX와는 무슨 차이가 있을까?

# 기본 사용법

```javascript
const headers = new Headers(); // 헤더 컨테이너
const init = {
  // 각종 옵션 설정
  method: "POST",
  headers,
  mode: "cors",
  cache: "default",
};
const request = new Request(URL, init);

fetch(request)
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.error(err);
  });
```

`fetch(URL, init)` 처럼 `Request` 객체를 전달하지 않고 URL과 init을 전달하는 방법도 있음. `Request` 객체를 이용하면 `init` 객체를 변화시켜 `Request`를 재사용할 수 있도록 복사함.

```javascript
const newRequest = new Reuqest(request, newInit);
```

## ✋주의사항

### `fetch()`가 반환하는 `Promise` 객체는 HTTP Error 상태를 reject하지 않음

네트워크 장애나 요청이 완료되지 못한 경우에 reject.

### 보통 `fetch`는 쿠키를 보내거나 받지 않음

쿠키를 전송하려면 자격증명(Credentials) 옵션을 반드시 설정해야함.

# 참고자료

[Using Fetch - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Fetch%EC%9D%98_%EC%82%AC%EC%9A%A9%EB%B2%95)
