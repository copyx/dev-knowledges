# JSX

Javascript를 확장한 문법. 리액트 엘리먼트를 생성. Javasript 안에서 마크업 사용 가능.

JSX의 중괄호(`{}`) 안에 모든 자바스크립트 표현식 사용 가능.

JSX는 자바스크립트에서 표현식으로 취급.

JSX에서 요소에 삽입되는 모든 값은 렌더링 전에 문자열로 변환됨. (XSS 공격 방지)

JSX의 마크업에서 `class` 속성은 `className`으로 바꿔서 사용 가능. 자바스크립트의 약속어 `class`와 혼동을 피하기 위함.

## JSX 변환 과정

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
