# Redux

> Redux is a predictable state container for JavaScript apps.<br/>
> 예측 가능한 상태 컨테이너

자바스크립트에서 사용하는 상태 관리 도구. 여러 다른 환경에서도 일관적으로 동작하는 앱을 만드는데 도움을 줌.

## Store

상태 저장 공간? 컨테이너? 같은 것. `createStore()`에 Reducer 함수를 넘겨주며 만듦.

## Reducer

상태 수정에 사용되는 함수. 오직 리듀서 안에서만 State 수정 가능.

## Dispatch

액션을 리듀서에 전달하는 메서드

## Subscribe

상태 변화가 있을 경우 실행되는 콜백을 등록하는 메서드
