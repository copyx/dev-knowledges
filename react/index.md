# React

"컴포넌트 방식 프론트엔드 프레임 워크"라고 알고 시작함. 공홈에서는 다음과 같이 설명.

> React는 사용자 인터페이스를 구축하기 위한 선언적이고 효율적이며 유연한 JavaScript 라이브러리입니다. “컴포넌트”라고 불리는 작고 고립된 코드의 파편을 이용하여 복잡한 UI를 구성하도록 돕습니다.

| Link                                    | Tags             |
| --------------------------------------- | ---------------- |
| [Component](Component.md)               | `react`          |
| [JSX](JSX.md)                           | `react`          |
| [create-react-app](create-react-app.md) | `react` `tool`   |
| [react-router](react-router.md)         | `react` `router` |
| [react-and-css](react-and-css.md)       | `react` `css`    |

## 리액트 기본 동작 방식

내가 작성한 리액트 컴포넌트를 VirtualDOM을 이용해 렌더링함.

## React Element

컴포넌트의 구성 요소. 불변객체라서 엘리먼트 생성 후 자식이나 속성을 변경할 수 없음. 마치 스냅샷처럼 요소의 특정 시점을 표현하고 있다고 생각하면 될 듯.

### Rendering

ReactDOM.render()를 이용해 렌더링. 렌더링 할 때 Root DOM Node를 지정해 그 안에 엘리멘트가 렌더링됨. 렌더링 함수가 여러번 호출되도 매번 모든 것을 새로 만들지 않고, 변경된 부분만 업데이트함.

## React.StrictMode

앱 내 잠재적인 문제를 알아내기 위한 도구. Fragment와 같이 UI를 렌더링하지 않으며, 자손들에 대한 부가적인 검사와 경고를 활성화함.

개발 모드에서만 활성화되기 때문에 프로덕션 빌드에는 영향을 끼치지 않음.

- 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
- 레거시 문자열 ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- 레거시 context API 검사

[참조: Strict 모드 - React](https://ko.reactjs.org/docs/strict-mode.html)
