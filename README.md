# nginx proxy of gitea
개인용 깃 서버를 윈도우 우분투에 git 설치해서 커맨드로 관리하다가 gitea로 변경하고자 했다. 접근 제한이나 인증서 관리를 좀 더 편하게 하고자 gitea용 nginx를 사용하기 위해 이 docker 컨테이너를 쓰기로 했다.

## Prerequisites - 전제조건
1. docker network를 하나 생성해야 한다. wonknet 으로 생성했고 gitea 컨테이너는 172.18.0.2로 설정해 두었다.

    ```bash
    docker network create --subnet=172.18.0.0/16 wonknet
    ```

2. gitea가 떠 있어야 한다. 이것 역시 docker 컨테이너이다.

## hosts 파일 수정

macos의 경우 /private/etc 경로에 hosts 파일이 위치한다.

```bash
sudo vi /private/etc/hosts
```

외부에서 접속이 안되므로 도메인의 아이피를 지정해 주자.

```text
127.0.0.1 wonkgitea.io
```

## 사설 인증서 생성

proxy/ssl 경로 안에서 openssl을 이용해서 사설 인증서를 만들자

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.crt
```

## nginx 시작

```bash
docker-compose up -d
```

## 컨테이너 확인

```bash
docker-compose ps -a 
```

## nginx 종료

```bash
docker-compose down
```
