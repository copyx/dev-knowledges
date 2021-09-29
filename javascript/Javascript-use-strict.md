# Javascript "use strict"

## 만들어진 이유

자바스크립트 하위 호환 문제를 해결하기 위해 만들어짐.

자바스크립트는 기존 기능 변경 없이 발전해옴. 그러나 2009년 ES5에 기존 기능 중 일부가 변경되었고, 하위 호환성 문제를 피하기 위해 변경사항 대부분을 엄격 모드에서만 활성화되도록 함.

## 사용법

### 스크립트 최상단에 "use strict" 지시자 표시

```javascript
"use strict"; // 'use strict'도 됨.

const a = ...
```

스크립트 전체가 "모던한" 방식으로 동작.

### 함수 본문 맨 앞에 "use strict" 지시자 표시

```javascript
function useStrictTest() {
    "use strict";

    console.log("test");
    ...
```

이 함수만 엄격 모드로 실행됨.

---

### 주의사항

- 최상단이 아니면 엄격 모드가 활성화되지 않을 수 있음.
- "use strict" 위에는 주석만 사용 가능.
- 활성화 후 취소 불가

## 브라우저 콘솔

기본적으로 비활성화 되어있음. `shift + enter`로 줄 바꿈을 하던가, 즉시 실행 함수를 이용해야함.

## 클래스와 모듈

클래스와 모듈을 사용하면 "use strict"를 자동 적용함.

# 참고자료

[엄격 모드 - JAVASCRIPT.INFO](https://ko.javascript.info/strict-mode)
