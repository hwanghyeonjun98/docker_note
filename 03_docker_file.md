# Docker file

1. DockerFile 기본 문법
    - 명령은 주로 대문자로 작성하고 인수는 상관 없음
         ```Docker
         [명령] [인수]
         ```

### Docker File 주요 명령

| 명령어        | 설명                                                                                                                                                                                                |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| FROM       | 베이스 이미지 지정 명령 (ex: `FROM httpd:alpnie`)                                                                                                                                                           |
| LABEL      | 버전 정보, 작성자 같은 이미지 설명을 작성하기 위한 명형 (ex: `LABEL version="1.0.0"`)                                                                                                                                    |
| CMD        | Docker container가 시작할 때 실행하는 쉘 명령을 지정하는 명령, RUN과 비슷하지만,</br> RUN은 이미지 작성시 실행하는 명령이고, CMD 명령어는 container를 시작할 떄 실행하는 명령임</br> (ex: `CMD ['python', 'app.py'])`                                     |
| RUN        | 쉫 명령을 실행하는 명령 (ex: `RUN ["apt-get", "install", "nginx"]`) </br> RUN은 이미지 작성시 실행되며, 일종의 새로운 이미지 layer를 만드는 역할을 함                                                                                   |
| ENTRYPOINT | Docker container가 시작할 때 실행하는 쉘 명령을 지정하는 명령</br> `docker run`커멘드 실행시 별도 명령어도 넣을 수 있는데, 이 때 CMD 명령어는 해당 명령으로 덮어씌어짐.</br> ENTRYPOINT로 지정한 명령은 `docker run` 커맨드 실행시 함꼐 넣어진 별도 명령어가 있더라도 덮어씌어지지 않고 실행됨 |
| EXPOSE     | Docker container 외부에 오픈할 포트 설장 (ex: `EXPOSE 8080)                                                                                                                                                 |
| ENV        | Docker container 내부에서 사용할 환경 변수 지정 (ex: `ENV PATH /usr/bin:$PATH`)                                                                                                                                |
| WORKDIR    | Docker container에서의 작업 디렉토리 설정                                                                                                                                                                    |
| COPY       | 파일 또는 디렉토리를 Docker container에 복사 ADD 와 달리 URL은 지정할 수 없으며, 압축 파일을 자동으로 풀어주지 않음 (ex: `COPY test.sh /root/test.sh`)                                                                                  |

| 명령      | 설명                                                                                                                                                                                                                                                                             |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ADD     | ADD와 COPY 명령은 유사하지만, <b>COPY 명령이 보다 명시적이므로, COPY 명령을 사용하도록 함</b></br> 파일, 디렉토리, 또는 특정 URL의 데이터를 Docker 이미지에 추가 (ex: `ADD file /var/www/html`)</br> 추가 할 파일이 tar압축 파일일 경우 자동으로 압축 풀어 줌</br> 동일한 이름의 파일 또는 디렉토리가 이미 Docker 이미지에 있을시에는 덮어씌우기 않음 (ex: `ADD test.sh /root/test.sh`) |
| SHELL   | 쉘 프로그램 지정 명령이지만 CMD 등으로 대체 가능하므로 참고 (ex: `SHELL ['/bin/bash', '-c']`)                                                                                                                                                                                                          |
| ARG     | dockerfile 내에서 필요한 변수 설정, docker 이미지/컨테이너에서 사용하는 환경 변수를 설정하는 ENV와 달리. dockerfile 스크립트 작성을 위해 필요한 변수 설정 (ex: `ARG env=dev`)                                                                                                                                                     |
| USER    | docker 이미지 및 컨테이너에서 작업 하는 사용자 ID 지정 (ex: `USER jjun`)                                                                                                                                                                                                                          |
| ONBUILD | 생성한 이미지를 기반으로, 새로운 이미지를 생성시 실행하는 명형을 지정 (ex: `ONBUILD ADD myweb.tar /var/www/html`)                                                                                                                                                                                            |

- 주석
    - #을 쓰면 주석
      ```Docker
      # Docker 주석입니다.
      ```

####  * dockerfile 생성시 반듯이 Dockerfile로 만들어야함(확장자 없음)

### FROM

- 베이스 이미지 지정 명령
- 반듯이 Dockerfile에 작성해야 하는 명형

```Docker
FROM alpine
```

### Dockerfile로 이미지 작성

```Docker
docker build [options] Dockerfile_dir
```

- 주요 옵션

| 옵션          | 설명                                                                                                                                                                                                             |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -t or --tag | 이미지 이름 설정. 이미지 이름은 저장소(DockerHub ID)/"이미지 이름:태그"와 같이 작성 가능</br>(저장소 이름 및 태그 이름은 작성 안해도 되며, 태그 이름이 없을 시 디폴트로 latest로 붙여짐)                                                                                       |
| -f          | 이미지 빌드시 디폴트로 Dockerfile 파일명으로 된 파일을 찾아서 이미지를 빌드함. 그 외의 파일명으로 이미지를 빌드할 경우 해당 옵션을 사용해서 파일명을 지정할 수 있음.                                                                                                            |
| --pull      | FROM으로 지정된 이미지는 한번 다운로드 받으면, 이미지 생성시 마다 새로 다운받지 않고 다운로드 받은 이미지를 사용함.</br> 해당 옵션은 이미지 생성시마다 새로 다운받으라는 옵션임. `--pull=true`와 같이 작성하며, 사용함.</br> DockerHub에 베이스 이미지를 수시로 업데이트하고, 이를 기반으로 새로운 이미지 생성시 자주 사용할 수 있는 옵션 |

```Docker
docker build [-t|--tag] imageName[:tag] [-f] [Dockerfile|/dir/fileName] [--pull=true]
```

### LABEL

- `LABEL` `key="value"`형식으로 작성
- 보통 저자, 버전, 설명, 작성 일자 들 각각 key 이름을 정하고 값을 넣는 경우가 있음
```Docker
FROM alpine

LABEL maintainer="jjun@abc.com"
LABEL version="0.0.1"
LABEL description="test"
```

### COPY
- 이미지에 로컬 디렉토리를 호스트 이미지 디렉토리에 복사
> `VOLUME` 명령도 있지만, 호스트 PC의 특정 디렉트로리를 컨테이너 내부 디렉토리에 연결하는 옵션은 `-v`옵션으로만 가능 하며.</br> `VOLUME`명령은 컨테이너 내부의 특정 디렉토리를 위한 볼륨을 생성하기 위해서만 사용
> 
> 예1: `VOLUME /myweb`
> 예1: `VOLUME ["/mydata", "mydata2"]`

```Docker
COPY [Local Directory Path] [Host Directory Path]
```
- 이미지 조사하기
  - `docker inspect [image name]`을 사용하면 이미지를 확인 정보를 확인할 수 있다.


### CMD
- CMD는 3가지 방법으로 작성 가능
- 반듯이 쌍따음표("")를 사용해야함

1. 리스트처럼 작성하기

````Docker
CMD ["executable", "param1", "param2", ...]

CMD ["/bin/sh", "-c", "echo", "hello"]
````
2. ENTRYPOINT 명령어에 인자를 설정하여 리스트처럼 장성하기

```Docker
CMD ["param1", "param2", ...]
```

3. 쉘 명령어처럼 작성하기

```Docker
CMD <commend> <param1> <param2> ...
```

> CMD는 Dockerfile에 하나만 작성하기! 뒤에 더 작성하면 뒤에 CMD 명령으로 덮어씌어짐