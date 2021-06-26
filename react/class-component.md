# Class Component

클래스 형태로 사용하는 컴포넌트. State, Life-cycle 메서드가 있음.

## Fuction Component에서 Class Component로 변경하는 5단계

1. `React.Component`를 확장하는 동일한 이름의 ES6 class를 생성합니다.
1. `render()`라고 불리는 빈 메서드를 추가합니다.
1. 함수의 내용을 `render()` 메서드 안으로 옮깁니다.
1. `render()` 내용 안에 있는 `props`를 `this.props`로 변경합니다.
1. 남아있는 빈 함수 선언을 삭제합니다.

## 부가 기능

- Local State
- Lifecycle Method

## Local State

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

### `setState()` 주의사항

1. 직접 State 수정 금지!<br/>
   `this.state` 말고 `setState()`로 수정할 것.
2. State 업데이트는 비동기적일 수 있음!<br/>
   `setState()` 내에서 `this.props`, `this.state` 사용하지 말 것. 대신 함수형 인자(`(state, props) => {}`)를 사용!
3. State 업데이트는 병합됨!<br/>
   `setState()`에 들어가는 객체 인자는 State에 얕게 병합됨. 넘겨준 객체에 있는 속성들만 값이 대체됨.

[참조: State를 올바르게 사용하기 - React](https://ko.reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)

## Lifecycle Methods

컴포넌트의 마운트, 언마운트, 업데이트, 에러 같은 상황이 발생함. 생명주기 메서드는 이런 컴포넌트의 이벤트마다 실행되는 메소드임. 이를 오버라이딩하면 원하는 타이밍에 원하는 코드를 실행시킬 수 있음.

[참조: 컴포넌트 생명주기 - React](https://ko.reactjs.org/docs/react-component.html#the-component-lifecycle)
