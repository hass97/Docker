# Docker network

## 요약
- 도커의 경우는 실행시 machine 과는 분리된 독자적인 network 를 형성한다.
- 도커 간의 연결이 필요한 경우 (동일 machine 위에) network 를 만들어 줘야 한다.
  - network 를 만든 후 도커들을 해당 network 에 connect 를 시켜주어야 한다.
  - 이때 도커 컨테이너의 이름이 호스트명으로 취급을 받는다.

### Commands

#### Network 명령어
```shell
# 네트워크 생성
$ docker network create <network 이름>

# 컨테이너 네트워크 연결
$ docker network connect <네트워크 이름> <컨테이너 명>

# 네트워크 삭제
$ docker network rm <네트워크 이름>

# 네트워크 표시
$ docker network ls
```
