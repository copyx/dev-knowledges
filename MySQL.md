# MySQL

[MySQL 개발자 페이지](https://dev.mysql.com/)

## 설치 및 기본 사용법

### 설치

맥OS용 패키지로 설치하려 했으나 다운로드 경로를 못찾아서 `brew`로 설치

```bash
$ brew install mysql # 최신버전. 특정 버전은 뒤에 @5.5 포맷으로 버전 붙이면 됨
```

### 초기 설정

설치하면 이렇게 안내가 나옴

```text
==> mysql
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
```

쉘에 `mysql_secure_installation` 명령어를 실행하면 설정 시작.

- 비밀번호 복잡하게 할래?
- 비밀번호 뭐로 할래?
- anonymous 사용자 지울래?
- 원격에서 root 로그인하는거 막을래?
- 테스트 데이터베이스 지울래?
- 변경된 권한을 테이블에 적용할래?

** `mysql_secure_installation`을 실행했는데 오류가 난다?**

```text
Error: Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

MySQL 서버와 연결이 안된다고 한다. 원인은 여러가지인데, 나는 MySQL 서버를 실행시키지 않은 상태였다.

```bash
brew services start mysql
```

### 접속

```bash
mysql -u root -p
Enter password:
```

옵션으로 host(-h), port(-P) 등 추가 가능.

## MySQL의 구조

![MySQL 기본 구조](mysql-structure.png)

MySQL에서는 Database와 Schema를 동의어로 사용함.

> [CREATE DATABASE creates a database with the given name. To use this statement, you need the CREATE privilege for the database. CREATE SCHEMA is a synonym for CREATE DATABASE.](https://dev.mysql.com/doc/refman/8.0/en/create-database.html)

## 데이터베이스 관련 명령

### 생성

```sql
CREATE DATABASE <database-name>
```

### 조회

```sql
SHOW DATABASES
```

### 사용

```sql
USE <database-name>
```

### 삭제

```sql
DROP DATABASE <database-name>
```

## SQL이란?

**S**tructured **Q**uery **L**anguege

구조화된 질문 언어. 읽히는대로 풀어보면 구조가 잘 짜여진 질문을 하기위한 언어 정도. 데이터베이스에 질문하기위한, 정보를 요청하기 위한 언어.

# 참고자료

[DATABASE2 - MySQL - 생활코딩](https://opentutorials.org/course/3161)
