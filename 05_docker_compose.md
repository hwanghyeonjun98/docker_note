# Docker Compose

## Docker Compose?

- 여러 컨테이너를 모아서 관리
- 웹서비스는 프론트엔드 서버, 데이터 베이스 서버, 백엔드 서버로 이루워져 있는 경우가 많음
    - 각각을 별도의 컨테이너로 작성하고, 연결하여 동작하기 때문에, Docker Compose 같은 컨테이너 관리 툴이 필요
- 서비스 규모가 커지면, 복수의 컨테이너를 유지보수 해야하며, 이를 위해 쿠버네티스 등의 툴이 사용됨

## Docker Compose 작성

- Docker Compose는 docker-compose.yml 파일을 작성, 실행
- .yml은 YAML(야멜이라고 부름)형식으로 작성함

## 참고

### YAML

- key: value 형식으로 작성
- 들여쓰기 중심 문법

### 기본 문법

- `#`: 해당 라인 주석처리
- `---`: 문서 시작을 나타냄(옵션)
- `...`: 문서 끝을 나타냄(옵션)
- `key: value`: key에 대한 값(value)
- 들여쓰기 : 2칸 또는 4칸
- `- [key | value]`: 리스트 표시
- 자료형
    - int, string, boolean 지원
        - int_type: 1
        - string_type: "string"
        - boolean_type: true | false
- 예시

```yaml
# 문서의 시작
---
jjun:
    -   name: hwang hj
    -   job:
            - frontend
            - backend
```

#### 참고 : 줄바꿈

- `|` 를 쓰면 마지막 줄바꿈을 포함
- `|-`를 쓰면 마지막 불바꿈 제외
- `>`를쓰면 중간에 있는 줄바꿈 무시함(마지막은 포함)

## Docker Compose 사용법

- Docker Compose는 Dockerfile에서 익힌 명령어 기반
- 기본적으로 아래 4가지의 큰 카테고리로 작성
    - 보통 version과 services만 설정하여 많이 사용함
    - volumes는 각 컨테이너 설정에서 volumes로 선언, network는 컨테이너간 네트워크 분리를 위한 추가 설정

```yaml
# Docker compose 파일 포맷 버전 설정
version: "3"

# 컨테이너 설정
services:

# 컨테이너에서 상용하면 volume 설정으로 대체 가능(욥션)
volumes:

# 컨테이너간 네트워크 분리를 위한 추가 설정(옵션)
network:
```

### version

- Docker Compose 파일 포맷 버전 지정
- docker버전에 따라 지원하는 Docker Compose 버전이 있으며, 기본적으로는 3버전으로 사용하는 것이 일반적임

### services

- 위 항목 아래에서 여러개 또는 하나의 컨테이너를 설정

### image

- 다음 코드에서 db는 컨테이너 이름을 정의한 것
- db라는 이름의 컨테이너 작성시 Docker Hub에 있는 이미지 설정

```yaml
services:
    db:
        image: mysql:5.8 # image[:tag]
```

### restart

- 컨테이너가 다운되었을 때, 항상 재시작 설정

```yaml
services:
    db:
        image: mysql:5.8
        restart: always
```

### volumes

- docker run 옵션 -v와 같은 역할
- 여러 volume 지정 가능함(리스트로 작성)

```yaml
services:
    db:
        image: mysql:5.8
        restart: always
        volumes:
            - ./mysqldata:/var/lib/mysql # [host path]:[container path]
```

### environment

- Dockerfile의 ENV 옵션과 동일

```yaml
services:
    db:
        image: mysql:5.8
        restart: always
        volumes:
            - ./mysqldata:/var/lib/mysql
        environment:
            - MYSQL_ROOT_PASSWORD=123456789
            - MYSQL_DATABASE=jjun
```

- env_file 옵션을 이용해 환경변수 파일을 읽어 들여올 수 있음

```yaml
services:
    db:
        image: mysql:5.8
        restart: always
        volumes:
            - ./mysqldata:/var/lib/mysql
        env_file:
            - ./mysql_env.env
```

### port

- docker run -p와 동일
- YAML 문법에서는 aa:bb형식이면 시간으로 인식하므로 "aa:bb"로 작성

```yaml
services:
    db:
        image: mysql:5.8
        restart: always
        volumes:
            - ./mysqldata:/var/lib/mysql
        environment:
            - MYSQL_ROOT_PASSWORD=123456789
            - MYSQL_DATABASE=jjun
        port:
            - "3306:3306"
```

### build

- Dockerfile을 별도 만들어서 사용 가능함

```yaml
build:
    context: Dockerfile directory
    dockerfile: Dockerfile
```

### links

- 컨테이너 연결

```yaml
links:
    - "db:mysql"
```

### container_name

- 컨테이너 이름 설정

```yaml
container_name: [ container name ]
```

- `docker-compose up --build -d`로 실행 했을 때 컨테이너 이름 확인할 수 있음

### depends_on

- 컨테이너를 생성하기전 먼저 생성 되어야 할 컨테이너를 설정

## Docker Compose 실행 / 중지

- 보통 -d 옵션을 사용하며 -d 옵션은 백그라운드 실행을 의미
- 실행

```Docker
docker-compose up -d
```

- 이미지 재빌드가 필요하면 `--build` 옵션 추가

```Docker
docker-compose up --build -d
```

- 종료

```Docker
docker-compose stop
```

- 삭제

```Docker
docker-compose down
```

- 로그 확인
    - 각 컨테이너의 모든 로그 확인

```Docker
docker-compose logs
```

- 설정확인
    - 실행 중인 docker compose의 docker-compose.yml 설정 확인

```Docker
docker-compose config
```

- 컨테이너 실행
    - 실행 중인 컨테이너에 명령어 실행

```Docker
docker-compose exec
```

- 테스트

```Docker
# docker compose 실행
docker-compose up -d
#실행중인 컨테이너 확인
docker ps 

# 실행 확인 후

# 컨테이너 삭제
docker-compose down
# 컨테이너 확인
docker ps
```

## .dockerignore 파일

- .gitignore와 같은 파일
- 파일 포맷

```Docker
# 주석
*/flask*
*/*/flask*
flask?
flask*
```

- `*/flask*`: 현제 디렉토리의 어떤 하위 디렉토리은 flask로 시작하는 디렉토리나 파일명 제외
- `*/*/flask*`: 현테 디렉토리 하위 디렉토리의 하위 디렉토리에서 flask로 시작하는 폴더나 파일명은 제외
    - 깊이 관계 없이 쓸려면 `**` 사용
- `flask? | flask*`: ?은 한글자를 의미, flaska나 flaskb로 디렉토리 제외, * 모든 문자열 의미, flask로 시작하는 모든 디렉토리나 파일을 제외
- `!`를 쓰면 해당 조건은 제외조건을 의미