# MySQL

[MySQL 개발자 페이지](https://dev.mysql.com/)

## 설치

맥OS용 패키지로 설치하려 했으나 다운로드 경로를 못찾아서 `brew`로 설치

```bash
$ brew install mysql # 최신버전. 특정 버전은 뒤에 @5.5 포맷으로 버전 붙이면 됨
```

## 초기 설정

쉘에 `mysql_secure_installation` 명령어를 실행하면 설정 시작.

- 비밀번호 복잡하게 할래?
- 비밀번호 뭐로 할래?
- anonymous 사용자 지울래?
- 원격에서 root 로그인하는거 막을래?
- 테스트 데이터베이스 지울래?
- 변경된 권한을 테이블에 적용할래?

## 접속

```bash
$ mysql -u root -p
Enter password:
```

옵션으로 host(-h), port(-P) 등 추가 가능.
