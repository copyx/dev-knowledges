# Function Component

함수 형태로 사용하는 컴포넌트. 클래스 컴포넌트와 달리 State, 생명주기 메서드가 없지만 Hook 사용 가능.

## State Hook

클래스 컴포넌트의 State와 같은 역할.

```jsx
// State Hook 사용법
import React, { useState } from "react";
function Example() {
  const [date, setDate] = useState(new Date());
}
```

`useState()`에 인자로 State의 초기값을 넘겨주면, `[state 변수, 갱신 함수]` 반환.

주의할 점은 클래스 컴포넌트의 **`setState()`와 달리 State 갱신 시 병합이 아닌 대체한다는 점!**

## Effect Hook

클래스 컴포넌트의 생명주기 메서드와 같은 역할. 첫 번째 렌더링과 이후의 모든 업데이트에서 수행됨.

Effect라는 이름은 Side Effects에서 따옴. React 컴포넌트 안에서 데이터 취득, DOM 직접 조작 등을 Side Effects라고 부름. 왜냐하면 이 작업들이 다른 컴포넌트에 영향을 줄 수도 있고, 렌더링 과정에서는 구현할 수 없는 작업이기 때문.

```jsx
// Effect Hook 사용법
import React, { useEffect } from "react";
function Example() {
  useEffect(() => { ... });
}
```

`useEffect()` 훅에 전달하는 함수를 effect라고 부름. effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장함. 그리고 effect는 브라우저의 화면 업데이트를 차단하지 않음. 대부분의 effect는 동기적으로 실행될 필요가 없음. 동기적 실행이 필요한 경우에는 `useLayoutEffect()`라는 훅이 있음.

```jsx
// Clean-up
import React, { useEffect } from "react";
function Example() {
  useEffect(() => {
    const id = subscribe();
    return () => {
      unsubscribe(id);
    };
  });
}
```

구독/해지처럼 정리(Clean-up)가 필요한 경우 effect 함수에서 정리 함수를 반환. 해당 함수는 **마운트 해제**시점에 실행됨. 단, effect가 렌더링마다 실행되는 것처럼 정리도 여러번 실행됨.

```jsx
// Effect Hook 건너뛰기
import React, { useEffect } from "react";
function Example() {
  useEffect(() => { ... }, [something]);
}
```

모든 렌더링 이후에 effect/clean-up 실행이 성능저하를 일으킬 수도 있음. `useEffect()`에 두 번째 인자로 값이 같은지 체크할 변수들을 넣어주면 해결됨. 리액트는 배열 내 모든 값을 다음 렌더링 때 비교해서 값이 같을 경우 effect를 건너뜀.

딱 한 번만 실행하고 싶다면 빈 배열(`[]`)을 전달하면 됨.

## Callback Hook

함수 컴포넌트 내 함수를 선언하면 매번 재선언 됨. 이 때문에 생기는 불필요한 렌더링을 줄이기 위해 메모이제이션이 되도록 Callback 훅 사용.

```jsx
// 사용법
import React, { useCallback } from "react";
export default function Example({ a }) {
  const onClick = useCallback(
    (e) => {
      console.log(a, e);
    },
    [a]
  );
  return <div onClick={onClick}>Example</div>;
}
```

콜백 안에서 참조되는 모든 값은 의존성 배열에 있어야함. 리액트에서는 이를 자동으로 생성하는 것이 목표인 것으로 보임.
