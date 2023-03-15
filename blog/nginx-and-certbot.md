# Nginx와 certbot으로 SSL 인증서 셋팅하기

## 1. Nginx 설치

```sh
sudo apt install nginx
```

## 2. 80포트가 열려있고, 도메인과 연결되어있는지 확인

포트 개방 여부는 `netstat -an` 명령으로 포트 확인 가능.

도메인은 브라우저에서 인증서로 인증할 도메인에 HTTP로 접속해보며 확인 가능.

## 3. certbot & nginx plugin 설치

```sh
sudo apt install certbot python3-certbot-nginx
```

## 4. certbot 실행

```sh
sudo certbot --nginx
```

실행하고 필요한 정보(email, domain 등) 입력하면 끝.

갱신할 필요도 없다. 알아서 crontab이나 systemd timer를 이용해서 갱신해준다.

## 참고자료

https://certbot.eff.org/instructions : 서버 소프트웨어 종류별, OS별 가이드 있음.
