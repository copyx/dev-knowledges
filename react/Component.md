# Component

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

## Props

사용자 정의 컴포넌트로 만든 엘리먼트에서 JSX 어트리뷰트와 자식을 모아 Props라는 단일 객체로 전달.

**모든 리액트 컴포넌트는 자신의 Props를 다룰 때 반드시 순수 함수처럼 동작해야 함.** 순수 함수는 입력값을 바꾸려하지 않으며, 같은 입력에 같은 출력을 반환함.

### 특수 Props 경고(번역)

JSX 엘리멘트에 있는 대부분의 props는 컴포넌트로 전달되지만, 두 특수 props(`ref`와 `key`)는 리액트에 의해 사용되며, 이로 인해 컴포넌트로 전달되지 않는다.

예를들어 컴포넌트에서 `this.props.key`에 대한 접근 시도(render 함수나 propTypes 같은)는 정의되지 않는다. 만약 자식 컴포넌트 안에서 같은 값에 접근할 필요가 있다면, 이 값을 다른 prop로 전달해야한다(예: `<ListItemWrapper key={result.id} id={result.id} />). 이게 중복처럼 보일 수 있지만, 재조정 힌트로부터 앱로직을 분리하는데 중요하다.

[참조: Special Props Warning - React](https://reactjs.org/warnings/special-props.html)

## State

Props와 유사하지만 비공개이며 컴포넌트에 의해 완전히 제어됨. Class Component에서는 Local State, Funcion Component에서는 Hook 사용.

### State 끌어올리기

React App. 내 변경이 일어나는 데이터는 **"진리의 원천(Source of Truth)"**를 하나만 둬야한다고 함.
이는 서로 다른 컴포넌트에 있는 State를 동기화시키는 것 보다 하향식 데이터 흐름에 기대는 것을 추천하기 때문.

State를 끌어올리는 작업은 양방향 바인딩 접근 방식보다 더 많은 **"보일러 플레이트"** 코드를 유발하지만, **버그를 찾고 격리하기 더 쉽게 만든다는 장점이 있음**.

https://ko.reactjs.org/docs/lifting-state-up.html#lessons-learned

## [Class Component](class-component.md)

클래스 형태로 사용하는 컴포넌트. State, Life-cycle 메서드가 있음.

## [Function Component](function-component.md)

함수 형태로 사용하는 컴포넌트. 클래스 컴포넌트와 달리 State, 생명주기 메서드가 없지만 Hook 사용 가능.
