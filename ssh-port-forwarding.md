# SSH Port Forwarding (이하 SSH 포트포워딩)

다른 말로 SSH Tunneling(이하 SSH 터널링)이라고 부름. SSH 터널은 연결 수립 뒤 외부로부터 데이터를 보호할 수 있는 SSH 클라이언트와 SSH 서버 사이의 연결통로. 이 터널을 이용해 프록시랑 비슷한 역할을 수행. SSH 포트를 통해서 다른 포트에 연결된 애플리케이션과 통신.

로컬 포트포워딩과 리모트 포트포워딩 두 가지가 있음.

```text
┌────────────┐
│ SSH Client │
└───║  ║─────┘
    ║  ║ SSH Tunnel
    ║  ║ Port 22    ╔══╗ Port 80 (Blocked)
┌───║  ║────────────║  ║──────┐
│┌──║  ║──────┐     ║  ║      │
││ SSH Server │     ║  ║      │
│└─────║  ║───┘     ║  ║      │
│      ║  ║    ┌────║  ║─────┐│
│      ║  ╚═════             ││
│      ╚════════ Application ││
│              └─────────────┘│
└─────────────────────────────┘
```

## 참고자료

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=alice_k106&logNo=221364560794
