# Docker commands

## 이미지
### 이미지 검색
```shell
# Docker hub 에 있는 이미지 검색
$ docker search ubuntu
```


### 이미지 받아오기 또는 저장하기 (Docker hub, ...)
```shell
# Docker image 를 Docker hub 으로 부터 받아오기
$ docker pull <이미지 이름>:<태그>
$ docker push <이미지 이름>:<태그>
```

### 이미지 출력
```shell
# 이미지 출력
$ docker images
```

### 이미지 삭제

```shell

$ docker rmi <이미지 이름>:<태그>
```

### 이미지 히스토리 살펴보기
- Dockerfile 에서 설정한 히스토리가 그대로 생성

```shell
$ docker history <이미지 이름>:<태그>
```

### 컨테이너 변경사항을 이미지로 생성

```shell
$ docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>
```

## 컨테이너

### 컨테이너 생성

- 옵션
  - -i(interactive) -t(Pseudo-tty) : Bash 셸에 입력 및 출력 가능
  - --name : 컨테이너 이름 지정

```shell
$ docker run <옵션> <이미지 이름> <실행할 파일>
```

### 컨테이너 목록 확인

- 옵션
  - -a : 현재 정지된 컨테이너까지 모두 출력
```shell
$ docker ps -a
```

### 컨테이너 실행 및 정지

```shell
# 컨테이너 실행
$ docker start <컨테이너 이름>

# 컨테이너 재실행
$ docker restart <컨테이너 이름>

# 컨테이너 정지
$ docker stop <컨테이너 이름>
```

### 컨테이너 접속
- Bash 쉘에서 exit 또는 Ctrl + D 를 입력하면 컨테이너가 정지됩니다.
- Ctrl + P, Ctrl + Q 를 차례대로 입력하여 컨테이너 정지 없이 컨테이너에서 빠져올 수 있음.


```shell
# /bin/bash 를 실행 해 놓은 상태라고 한다면 컨테이너는 명령을 넣을 수 있지만, 어플을 실행해 놓은 컨테이너라면 출력만 볼 수 있다.
$ docker attach <컨테이너 명>
```

### 컨테이너 명령 실행

```shell
$ docker exec <컨테이너 이름> <명령> <매개 변수>
```

### 컨테이너 삭제

```shell
$ docker rm <컨테이너 이름>
```

### 컨테이너 변경된 파일 확인

- 컨테이너가 실행되면서 변경된 파일 목록 출력
  - A : 추가된 파일
  - C : 변경된 파일
  - D : 삭제된 파일
```shell
$ docker diff <컨테이너명>
$ docker diff hello-nginx
```

## 추가 명령어

### 도커 내부 파일 꺼내기

```shell
$ docker cp <컨테이너 이름>:<경로> <호스트 경로>
$ docker cp hello-nginx:/etc/nginx/nginx.conf ./
```

### 세부 정보 확인
- 네트워크, 등 다양한 자세한 내용 확인

```shell
$ docker inspect <이미지 또는 컨테이너 이름>
```