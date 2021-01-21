# [React-redux](https://react-redux.js.org/)

React를 위한 공식적인 Redux UI 바인딩 라이브러리.

## Hooks

Functional Component와 함께 사용하기 위한 훅 API들. 훅을 사용하려면 `<Provider>` 컴포넌트로 앱 전체를 감싸야함.

```jsx
const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

### `useSelector()`

Redux store로부터 상태를 가져올 수 있는 API.

### `useDispatch()`
