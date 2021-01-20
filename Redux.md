# [Redux](https://redux.js.org/)

> Redux is a predictable state container for JavaScript apps.<br/>
> 예측 가능한 상태 컨테이너

자바스크립트에서 사용하는 상태 관리 도구. 여러 다른 환경에서도 일관적으로 동작하는 앱을 만드는데 도움을 줌.

## Redux에서 사용하는 요소들

```javascript
// Redux를 이용한 간단한 예시
import { createStore } from "redux";

const add = document.getElementById("add");
const minus = document.getElementById("minus");
const number = document.querySelector("span");

// Reducer
const countModifier = (state = 0, action) => {
  console.log(state, action);

  switch (action.type) {
    case "ADD":
      return state + 1;
    case "MINUS":
      return state - 1;
    default:
      return state;
  }
};

// Store
const countStore = createStore(countModifier);

// Subscribe
countStore.subscribe(() => {
  number.innerText = countStore.getState();
});

add.addEventListener("click", () => {
  // Dispatch
  countStore.dispatch(
    { type: "ADD" } // Action
  );
});
minus.addEventListener("click", () => countStore.dispatch({ type: "MINUS" }));
```

### Store

상태를 저장하는 공간이나 컨테이너 같은 것. `createStore()`에 Reducer 함수를 넘겨주며 만듦.

### Reducer

상태 수정에 사용되는 함수. 오직 Reducer 안에서만 State 수정 가능. Reducer가 반환하는 것이 State로 설정됨.

### Dispatch

Action을 Reducer에 전달하는 메서드

### Action

반드시 Plain Object이어야하며, `type` 속성을 필수적으로 가져야함.

### Subscribe

상태 변화가 있을 경우 실행되는 콜백을 등록하는 메서드

## Redux 3원칙

### Single source of truth

### State is read only

### Changes are made with pure functions
