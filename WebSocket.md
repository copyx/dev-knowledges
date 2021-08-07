# WebSocket

HTTP와 달리 양방향 통신이 가능한 웹 프로토콜.

## [ws: a Node.js WebSocket library](https://www.npmjs.com/package/ws)

Node.js를 위한 WebSocket 구현체 라이브러리. 브라우저에는 [WebSocket 표준 객체](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)가 이미 있음.

## 사용법

연결하고, 메시지를 주고 받는다. 이게 기본.

## [Socket.io](https://socket.io/)

WebSocket 구현체가 아님. WebSocket보다 무겁지만, 훨씬 더 다양한 기능들을 제공함. 기본적으로 실시간, 양방향, 이벤트 기반 통신을 제공하는 것은 같음.

실제로 써보니 진짜 편함!

- 클라이언트에서 서버의 API를 이용해 Socket.io 스크립트 파일을 가져오고, 이로인해 웹소켓처럼 서버의 URL을 설정할 필요가 없음.
- 커스텀 이벤트를 제공함. 메시지를 타입별로 처리하기위해 분기할 필요가 없음.
- JSON 데이터를 바로 보낼 수 있음.
