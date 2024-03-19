# Docker 조사하기

## docker history
- 이미지 히스토리 확인
```Docker
docker history [imageName]
```

## docker cp
- 컨테이너에서 특정 파일을 호스트PC로 가져오는 명력
- 반대로 호스트PC에서 컨테이너로 널을 수도 있음
```Docker
docker cp [containerName]:[container_path] [host_path]
```

- 덮어씌우기(반대로 입력하기)
```Docker
docker cp [host_path] [containerName]:[container_path]
```

## docker commit
- 컨테이너 변경사항을 이미지 파일로 생성
```Docker
docker commit [option] [containerID|containerNAME[:tag]]
```

- 예시
```Docker
docker commit - "add vim" mywebserver
```

## docker diff
- 컨에이너가 실행하면서 기본 이미지와 비교하여 변경된 파일 목록을 출력
```Docker
docker diff [containerID|containerNAME]
```

|기호| 설멸           |
|---|--------------|
|A| 파일또는 디렉토리 추가 |
|D| 파일또는 디렉토리 삭제 |
|C| 파일또는 디렉토리 변경 |


## --link
- 컨테이너 끼리 연결하기 위한 옵션
```Docker
--link containerName:conectContainerName
```