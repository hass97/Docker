# Dockerfile

## 문법
- 기본 형식 : <명령> <매개변수>

### FROM
- 어떤 이미지를 기반으로 이미지를 생성할지 설정

```yaml
# FROM <이미지>
# FROM <이미지>:<태그>
FROM ubuntu:14.04
```

### MAINTAINER
- 이미지를 생성한 사람의 정보를 설정 (형식은 자유)

### RUN
- FROM 에서 설정한 이미지 위에서 스크립트 혹은 명령을 실행한다.
- RUN 으로 실행한 결과는 캐시되며 다음 빌드 때 재사용됨.
- 캐시된 결과를 사용하지 않으려면 docker build 명령에서 --no-cache 옵션을 사용

```yaml
# RUN <명령>
# RUN ["<실행 파일>", "<매개 변수1>", "<매개 변수2>"]

RUN apt-get install -y nginx
RUN ["apt-get", "install", "-y" , "nginx"]
```

### CMD
- CMD 는 컨테이너가 시작되었을 때 스크립트 혹은 명령
- docker run 으로 컨테이너를 생성하거나 docker start 명령으로 정지된 컨테이너를 시작할 때 실행된다.

```yaml
# CMD <명령>
CMD touch /home/hello/hello.txt
CMD ["redis-server"]

# 쉘 스크립트 문법과 관련된 문자를 그대로 실행 파일에 넘겨 줄 수 있음
CMD ["mysqld", "--datedir=/var/lib/mysql", "--user=mysql"]
```

### ENTRYPOINT
- 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행합니다.
- docker run, docker start 로 실행됨
- Dockerfile 에서 단 한번만 사용 가능
- FROM 으로 설정한 이미지에 포함된 /bin/sh 실행 파일을 사용하며 실행 파일이 없으면 사용 불가

```yaml
ENTRYPOINT ["<실행 파일>", "<매개 변수>", "<매개 변수2>"]
```

### 도커 실행 시 CMD, ENTRYPOINT
- docker run 에서 이미지 다음에 실행할 파일을 설정할 수 있음
  - 설정 시 CMD 는 무시된다.
- docker run 명령에서 실행할 파일을 설정하면 ENTRYPOINT 는 무시되지 않고, 실행할 파일 설정 자체를 매개 변수로 받아서 처리합니다.
- --entrypoint 옵션으로 실행할 경우 Dockerfile 에서 설정한 ENTRYPOINT 는 무시된다.
```shell
$ docker run <이미지> <실행할 파일>
```

- FROM : 부모 이미지, 기존 이미지를 기반으로 생성 <이미지 이름>:<태그>
- RUN : 쉘 스크립트 혹은 명령을 실행
  - install 시 명령에 사용자 입력을 받을 수 없어 -y 옵션 사용 가능
  - -v 옵션으로 Volume 호스트와 디렉터리 공유 가능
- CMD : 컨테이너가 시작되었을 때 실행할 실행 파일 또는 셸 스크립트입니다.
- WORKDIR : CMD 에서 설정한 실행 파일이 실행될 디렉터리
- EXPOSE : 호스트와 연결할 포트 번호 (Docker run 에도 넣어 줄 수 있음)

## EXPOSE
- EXPOSE 는 호스트와 연결할 포트 번호를 설정
- docker run 명령의 --expose 옵션과 동일하다.
  - 해당 expose 를 사용하게 될 경우 외부에 노출이 되지는 않는다.
    - 외부에 노출을 시키고 싶을 경우에는 docker run 명령의 -p 옵션을 사용해야 한다.

```yaml
# Dockerfile
EXPOSE 80
EXPOSE 443
```

## ENV
- 환경 변수를 설정하고 해당 변수는 RUN, CMD, ENTRYPOINT에 적용된다.
- docker run 시에도 환경 변수 설정 가능 (-e) 옵션

```yaml
ENV <환경변수> <값>
ENV HELLO 1234
CMD echo ${HELLO}
```

```shell
$ docker run -e HELLO=4321 example
```

## ADD
- ADD는 파일을 이미지에 추가합니다.
- 복사할 파일 경로는 인터넷에 있는 파일의 URL 을 설정할 수 있습니다.
- <이미지에서 파일이 위치한 경로>는 항상 절대 경로로 설정해야 합니다. 마지막이 /로 끝나면 디렉터리가 생성되고 파일은 그 아래에 복사됩니다.
- 압축 파일의 경우는 해제하고 풀어서 추가합니다.

```yaml
ADD <복사할 파일 경로> <이미지에서 파일이 위치한 경로>
ADD http://example.com/hello.txt /home/hello/
```

## COPY
- 파일을 이미지에 추가합니다.
- 복사할 파일 경로는 디렉터리로 설정하게 되면 디렉터리의 모든 파일을 복사합니다.

```yaml
COPY <복사할 파일 경로> <이미지에서 파일이 위치한 경로>
COPY hello-entrypoint.sh /entrypoint.sh
```

## VOLUME
- VOLUME 은 디렉터리의 내용을 컨테이너에 저장하지 않고 호스트에 저장하도록 설정합니다.
- docker run 으로 설정이 가능 -v 옵션

```yaml
VOLUME <컨테이너 디렉터리>
VOLUME ["컨테이너 디렉터리 1", "컨테이너 디렉터리 2"]
```

```shell
$ docker run -v <호스트 디렉터리>:<컨테이너 디렉터리>
```

## USER
- 명령을 실행할 사용자 계정을 설정
- USER 뒤에 오는 모든 RUN, CMD, ENTRYPOINT

```yaml
USER <계정 사용자명>
```

## WORKDIR
- WORKDIR 은 RUN, CMD, ENTRYPOINT 의 명령이 실행될 디렉터리를 설정합니다.
- 상대 경로도 사용이 가능 (최초 기준은 / 입니다.)

```yaml
WORKDIR <경로>
WORKDIR /var/www
```

## ONBUILD
- ONBUILD는 생성한 이미지를 기반으로 다른 이미지가 생성될 때 명령을 실행(trigger)합니다.
- 다음번에 이미지가 FROM 으로 사용될 때 실행할 명령을 예약하는 기능
- ONBUILD 는 바로 아래 자식 이미지를 생성할 때만 적용되고, 손자 이미지에는 적용되지 않는다.

```yaml
ONBUILD <Dockerfile 명령> <Dockerfile 명령의 매개 변수>
ONBUILD RUN touch /hello.txt
```


