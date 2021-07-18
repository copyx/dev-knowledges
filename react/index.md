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

### 번들 사이즈 분석

create-react-app으로 프로젝트를 만들고 source-map-explorer 모듈을 이용해 번들 사이즈 분석 가능

```bash
yarn add source-map-explorer
```

source-map-explorer 설치 후 package.json의 scripts에 추가

```diff
   "scripts": {
+    "analyze": "source-map-explorer 'build/static/js/*.js'",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
```

프로젝트를 빌드한 후 분석

```bash
npm run build
npm run analyze
```

참고: https://create-react-app.dev/docs/analyzing-the-bundle-size/

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

#### State 끌어올리기

React App. 내 변경이 일어나는 데이터는 **"진리의 원천(Source of Truth)"**를 하나만 둬야한다고 함.
이는 서로 다른 컴포넌트에 있는 State를 동기화시키는 것 보다 하향식 데이터 흐름에 기대는 것을 추천하기 때문.

State를 끌어올리는 작업은 양방향 바인딩 접근 방식보다 더 많은 **"보일러 플레이트"** 코드를 유발하지만, **버그를 찾고 격리하기 더 쉽게 만든다는 장점이 있음**.

https://ko.reactjs.org/docs/lifting-state-up.html#lessons-learned

### [Class Component](class-component.md)

클래스 형태로 사용하는 컴포넌트. State, Life-cycle 메서드가 있음.

### [Function Component](function-component.md)

함수 형태로 사용하는 컴포넌트. 클래스 컴포넌트와 달리 State, 생명주기 메서드가 없지만 Hook 사용 가능.

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

## react-router

https://reactrouter.com/

서버와 통신 없이 클라이언트에서 페이지를 라우팅해주는 라이브러리? 도구?

페이지 간 이동 시 데이터 전달 및 패스 파라미터 처리도 해줌.

### Route - exact

라우터에 exact 설정이 되어있지 않으면 path의 앞 부분이 일치하는 경우 화면에 출력됨.

```jsx
<BrowserRouter>
  <Route path="/tv" component={TV} />
  <Route path="/tv/popular" exact render={() => <h1>Popular</h1>} />
</BrowserRouter>
// /tv/popular에 접근하면 둘 다 렌더링됨
```

### Redirect

from에서 to로 리다이렉트 해주는 컴포넌트

```jsx
<BrowserRouter>
  <Route path="/" exact component={Home} />
  <Redirect from="*" to="/" />
</BrowserRouter>
```

### Switch

자식 라우트 중 현재 패스에 여러 라우트가 해당해도 첫 번째만 렌더링하게 해주는 컴포넌트

```jsx
<BrowserRouter>
  <Switch>
    <Route path="/tv" component={TV} />
    <Route path="/tv/popular" render={() => <h1>Popular</h1>} />
  </Switch>
</BrowserRouter>
// /tv/popular에 접근하면 tv만 렌더링됨
```
