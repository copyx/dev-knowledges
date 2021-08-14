# WebRTC

Web Real-Time Communication

서버 없이 브라우저끼리 오디오, 비디오, 데이터를 교환할 수 있도록 하는 기술. 써드 파티 플러그인이나 소프트웨어 없이 Peer-to-peer로 데이터 교환 가능.

MDN의 설명에서는 Google의 Adapter.js 라이브러리 사용을 강력하게 고려해봐야한다고 말함.

- 참조: https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API

## ICECandidate

Internet Connectivity Establishment Candidate

## 같은 네트워크에 있지 않으면 서로를 못 찾음.

STUN 서버 필요. 내 기기의 공요 주소를 알려주는 서버.

## WebRTC를 쓰면 안좋을 경우

### Peer가 많아지는 경우 느려질 수 있음

같은 데이터를 Peer의 수 만큼 업로드해야함. Mesh topology에서는 연결의 수가 N^2. Star Topology는 N으로 훨씬 줄어듬. 업로드도 중앙에 한 번만하면 됨.
