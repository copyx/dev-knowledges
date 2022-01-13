# Docker

컨테이너 기반의 오픈소스 가상화 플랫폼

> [Container란?](container.md)

docker CLI는 docker host에 명령을 전달하고 결과를 받아서 출력하는 클라이언트/서버 구조

## Command

### `docker run`

컨테이너를 실행하는 명령어

|   options   |                    description                    |
| :---------: | :-----------------------------------------------: |
|    `-d`     |  detached mode (백그라운드에서 독립적으로 실행)   |
|    `-p`     |           호스트와 컨테이너의 포트 연결           |
|    `-v`     |         호스트와 컨테이너의 디렉토리 연결         |
|    `-e`     |       컨테이너 내에서 사용할 환경변수 설정        |
|  `--name`   |                컨테이너 이름 설정                 |
|   `--rm`    |        프로세스 종료 시 컨테이너 자동 제거        |
|    `-it`    | `--interactive` & `--tty` 터미널 입력을 위한 옵션 |
| `--network` |                   네트워크 연결                   |

```bash
# Ubuntu 20.04 버전에서 /bin/sh 실행 후 터미널 연결. 종료 후 컨테이너 제거
docker run --rm --it ubuntu:20.04 /bin/sh

docker run --rm -p 5678:5678 hashicorp/http-echo -text="hello"

# MySQL 띄우기
docker run -d -p 9906:3306 \
-e MYSQL_ALLOW_EMPTY_PASSWORD=true \
--name mysql \
mysql:5.7
```

### `docker exec`

실행중인 도커 컨테이너에 접속할 때 사용

```bash
docker exec -it mysql mysql # MySQL 접속
```

### `docker ps`

현재 실행중인 컨테이너들을 볼 수 있는 명령어

```bash
docker ps
docker ps -a
```

### `docker stop`

실행중인 컨테이너를 중지하는 명령어. 한 번에 여러개 중지 가능.

```bash
docker stop mysql
docker stop 38c0d5da6e1b 5e612c77a8a1 # CONTAINER ID로 중지
```

### `docker rm`

종료된 컨테이너를 완전히 제거하는 명령어

```bash
docker rm mysql
```

### `docker logs`

컨테이너의 로그를 확인하는 명령어

```bash
docker rm mysql -f # 계속 기다리며 보기
docker rm mysql --tail 10 # 마지막 10개 로그
```

### 이미지 관련 명령어

- `docker images`
  - 다운로드한 이미지를 확인하는 명령어
- `docker pull`
  - 이미지를 다운로드하는 명령어
- `docker rmi`
  - 이미지를 삭제하는 명령어(컨테이너 실행중인 이미지는 삭제되지 않음)

### 네트워크 관련 명령어

- `docker network create`
  - 네트워크 생성 명령
- `docker network connect`
  - 컨테이너를 네트워크에 연결하는 명령어
