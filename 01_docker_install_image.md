# Docker 설치 및 이미지 관련 명령어

## 1. docker 설치
- https://www.docker.com/get-started/

## 2. docker 명령어
1. 기본 명령어
```Docker
docker [명령] [옵션] [선택자(이미지ID/컨테이너 등...]
```
- 이미지 관련
```Docker
docker images [옵션] ...
```
- 컨테이너 관련
```Docker
docker container [옵션] ...
```

## 3. 이미지 다운
1. docker hub 가입
2. 로그인하기
```Docker
docker login

# 로그아웃
docker logout
```
3. 이미지 검색
- TAG는 docker hub 홈페이지에서 확인해야함
```Docker
docker search [검색어]
```
4. 이미지 다운
- tag가 없을 시 최신 이미지로 다운됨
```Docker
docker pull imageName[:tag]
```

* 다운받은 이미지 확인
```Docker
docker images

docker image ls

docker image ls -q  # -q : 이미지 ID만 출력
```

## 4. 이미지 삭제
- 이미지 REPOSITORY 이름 사용 시 tag까지 정확히 써줘야한다.
```Docker
docker rmi 이미지ID(이미지 REPOSITORY 이름)

docker image rm 이미지ID(이미지 REPOSITORY 이름)
```