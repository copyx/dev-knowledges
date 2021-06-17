# React

"컴포넌트 방식 프론트엔드 프레임 워크"라고 알고 시작함. 공홈에서는 다음과 같이 설명.

> React는 사용자 인터페이스를 구축하기 위한 선언적이고 효율적이며 유연한 JavaScript 라이브러리입니다. “컴포넌트”라고 불리는 작고 고립된 코드의 파편을 이용하여 복잡한 UI를 구성하도록 돕습니다.

## create-react-app

페북에서 만든 리액트 앱 초기 셋팅 도구.

```bash
$ npx create-react-app [app name]
```

위 명령어 하나면 프로젝트 폴더가 만들어지고 그 안에 리액트 셋팅이 끝!

- npx는 어떻게 내가 설치하지 않은 create-react-app을 알아서 설치해서 실행시켜줄까?

## 리액트 기본 동작 방식

내가 작성한 리액트 컴포넌트를 VirtualDOM을 이용해 렌더링함.

## Component

리액트 엘리먼트를 반환하는 함수. UI를 재사용 가능한 단위로 나눈 조각. **함수 컴포넌트**, **클래스 컴포넌트** 두 가지 종류 있음.

```jsx
// Function Component
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

함수 컴포넌트, 클래스 컴포넌트 둘 모두 몇 가지 추가 기능이 있음.

- **사용자 정의 컴포넌트는 반드시 대문자로 시작!**<br/>소문자로 시작하면 리액트가 HTML 태그 같은 내장 컴포넌트로 인식함.
- **컴포넌트 쪼개기를 두려워하지 말 것!**
- **props의 이름은 사용될 context가 아닌 컴포넌트 자체의 관점에서 지을 것!**

### Props

사용자 정의 컴포넌트로 만든 엘리먼트에서 JSX 어트리뷰트와 자식을 모아 Props라는 단일 객체로 전달.

**모든 리액트 컴포넌트는 자신의 Props를 다룰 때 반드시 순수 함수처럼 동작해야 함.** 순수 함수는 입력값을 바꾸려하지 않으며, 같은 입력에 같은 출력을 반환함.

#### 특수 Props 경고(번역)

JSX 엘리멘트에 있는 대부분의 props는 컴포넌트로 전달되지만, 두 특수 props(`ref`와 `key`)는 리액트에 의해 사용되며, 이로 인해 컴포넌트로 전달되지 않는다.

예를들어 컴포넌트에서 `this.props.key`에 대한 접근 시도(render 함수나 propTypes 같은)는 정의되지 않는다. 만약 자식 컴포넌트 안에서 같은 값에 접근할 필요가 있다면, 이 값을 다른 prop로 전달해야한다(예: `<ListItemWrapper key={result.id} id={result.id} />). 이게 중복처럼 보일 수 있지만, 재조정 힌트로부터 앱로직을 분리하는데 중요하다.

[참조: Special Props Warning - React](https://reactjs.org/warnings/special-props.html)

### State

Props와 유사하지만 비공개이며 컴포넌트에 의해 완전히 제어됨. Class Component에서는 Local State, Funcion Component에서는 Hook 사용.

### Class Component

#### Fuction Component에서 Class Component로 변경하는 5단계

1. `React.Component`를 확장하는 동일한 이름의 ES6 class를 생성합니다.
1. `render()`라고 불리는 빈 메서드를 추가합니다.
1. 함수의 내용을 `render()` 메서드 안으로 옮깁니다.
1. `render()` 내용 안에 있는 `props`를 `this.props`로 변경합니다.
1. 남아있는 빈 함수 선언을 삭제합니다.

#### 부가 기능

- Local State
- Lifecycle Method

#### Local State

클래스의 생성자에서 원하는 state 추가

```jsx
class Example extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    render() { ... }
}
```

State에 등록된 값 변경에는 `setState()` 사용.

##### `setState()` 주의사항

1. 직접 State 수정 금지!<br/>
   `this.state` 말고 `setState()`로 수정할 것.
2. State 업데이트는 비동기적일 수 있음!<br/>
   `setState()` 내에서 `this.props`, `this.state` 사용하지 말 것. 대신 함수형 인자(`(state, props) => {}`)를 사용!
3. State 업데이트는 병합됨!<br/>
   `setState()`에 들어가는 객체 인자는 State에 얕게 병합됨. 넘겨준 객체에 있는 속성들만 값이 대체됨.

[참조: State를 올바르게 사용하기 - React](https://ko.reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)

#### Lifecycle Methods

컴포넌트의 마운트, 언마운트, 업데이트, 에러 같은 상황이 발생함. 생명주기 메서드는 이런 컴포넌트의 이벤트마다 실행되는 메소드임. 이를 오버라이딩하면 원하는 타이밍에 원하는 코드를 실행시킬 수 있음.

[참조: 컴포넌트 생명주기 - React](https://ko.reactjs.org/docs/react-component.html#the-component-lifecycle)

### Function Component

함수 형태로 사용하는 컴포넌트. 클래스 컴포넌트와 달리 State, 생명주기 메서드가 없지만 Hook 사용 가능.

#### State Hook

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

#### Effect Hook

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

#### Callback Hook

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

## React Element

컴포넌트의 구성 요소. 불변객체라서 엘리먼트 생성 후 자식이나 속성을 변경할 수 없음. 마치 스냅샷처럼 요소의 특정 시점을 표현하고 있다고 생각하면 될 듯.

#### Rendering

ReactDOM.render()를 이용해 렌더링. 렌더링 할 때 Root DOM Node를 지정해 그 안에 엘리멘트가 렌더링됨. 렌더링 함수가 여러번 호출되도 매번 모든 것을 새로 만들지 않고, 변경된 부분만 업데이트함.

## JSX

Javascript를 확장한 문법. 리액트 엘리먼트를 생성. Javasript 안에서 마크업 사용 가능.

JSX의 중괄호(`{}`) 안에 모든 자바스크립트 표현식 사용 가능.

JSX는 자바스크립트에서 표현식으로 취급.

JSX에서 요소에 삽입되는 모든 값은 렌더링 전에 문자열로 변환됨. (XSS 공격 방지)

JSX의 마크업에서 `class` 속성은 `className`으로 바꿔서 사용 가능. 자바스크립트의 약속어 `class`와 혼동을 피하기 위함.

### JSX 변환 과정

```jsx
// JSX
const element = <h1 className="greeting">Hello, world!</h1>;
```

```javascript
/// Babel 트랜스파일 후
const element = React.createElement(
  "h1",
  { className: "greeting" },
  "Hello, world!"
);
```

```javascript
// React.createElement()로 객체 생성 후
const element = {
  type: "h1",
  props: {
    className: "greeting",
    children: "Hello, world!",
  },
};
```

## React.StrictMode

앱 내 잠재적인 문제를 알아내기 위한 도구. Fragment와 같이 UI를 렌더링하지 않으며, 자손들에 대한 부가적인 검사와 경고를 활성화함.

개발 모드에서만 활성화되기 때문에 프로덕션 빌드에는 영향을 끼치지 않음.

- 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
- 레거시 문자열 ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- 레거시 context API 검사

[참조: Strict 모드 - React](https://ko.reactjs.org/docs/strict-mode.html)

## react-router-dom

서버와 통신 없이 클라이언트에서 페이지를 라우팅해주는 라이브러리? 도구?

페이지 간 이동 시 데이터 전달 및 패스 파라미터 처리도 해줌.
