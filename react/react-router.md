# react-router

https://reactrouter.com/

서버와 통신 없이 클라이언트에서 페이지를 라우팅해주는 라이브러리? 도구?

페이지 간 이동 시 데이터 전달 및 패스 파라미터 처리도 해줌.

## Route - exact

라우터에 exact 설정이 되어있지 않으면 path의 앞 부분이 일치하는 경우 화면에 출력됨.

```jsx
<BrowserRouter>
  <Route path="/tv" component={TV} />
  <Route path="/tv/popular" exact render={() => <h1>Popular</h1>} />
</BrowserRouter>
// /tv/popular에 접근하면 둘 다 렌더링됨
```

## Redirect

from에서 to로 리다이렉트 해주는 컴포넌트

```jsx
<BrowserRouter>
  <Route path="/" exact component={Home} />
  <Redirect from="*" to="/" />
</BrowserRouter>
```

## Switch

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
