# Docker container 관련 명령어

## 1. 컨테이너 생성
1. 컨테이너 생성
```Docker
docker create [imageName[:tag]]
docker create --name imageName [imageName[:tag]]
```
2. 생성된 컨테이너 확인
   - 실행 중인 컨테이너 확인
        ```Docker
        docker ps
        ```
   - 전체 컨테이너 확인
        ```Docker
        docker ps -a  # -q를 사용하면 컨테이너 ID만 출력
     
        CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS    PORTS     NAMES
        80cfc7b0ba5f   ubuntu    "/bin/bash"   2 minutes ago   Created             compassionate_blackwell
        ```
3. 컨테이너 삭제
```Docker
docker rm containerName|containerID
```

4. 컨테이너 실행
```Docker
docker start containerName|containerID
```
- start 명령어는 싱행하면 바로 중지됨

5. docker run
- docker run 주요 옵션

| 옵션      | 설명                     |
|---------|------------------------|
| -i      | 컨테이너 입력을 열어 놓는 옵션      |
| -t      | 가상 터미널을 할당하는 옵션        |
| --name  | 컨테이너 이름 설정 옵션          |
| -d      | 컨테이너 백그라운드 실행 옵션       |
| --rm    | 컨테이너 종료 시 컨테이너 자동 삭제 옵션 |
| -p      | 호스트 컨테이너 포트 연결 옵션      |
| -v      | 호스트 컨테이너 디렉토리 연결 옵션    |

```Docker
docker run -it ubuntu

docker run -it --name myubuntu ubuntu

docker ps -a
```
- 실핼중인 컨테이너 접속
```Docker
docker attach containerName|containerID
```

6. 실행 중인 컨테이너 종료하기
```Docker
docker stop containerName|containerID
```
- 컨테이너를 잠깐 멈출려면 `docker pause containerName|containerID` 명령어 사용

7. 웹서버 docker run 옵션 테스트
- 테스트 이미지
  - apache
  - nginx

7.1. apache 웹서버 찾기
```Docker
docker search httpd
```

7.2. -p 옵션 예시
```Docker
docker -d -p 접속할 포트:호스트 포트 이미지이름
```

7.3 -v 옵션 예시

```Docker
docker -d -p 접속할 포트:호스트 포트 -v 로컬절대경로:호스트절대경로 이미지이름
```

8. docker가 사용하고 있는 용량 확인
```Docker
docker system df 
```

9. 실행 중인 컨테이너 사용 리소스 확인
```Docker
docker container stats 
```

10. 실행 중인 컨테이너 명령 실행하기
- 컨테이너가 샐행 중일 때만 명령을 실행할 수 있음
```Docker
docker exec [option] [containerID] [agrs]
``` 

11. 실행 중인 컨테이너 연결
```Docker
docker attach containerName|containerID
``` 

12. 모든 컨테이너 종료 및  삭제
```Docker
docker stop $(docker ps -a -q)

docker rm $(docker ps -a -q)

docker rmi $(docker images -q)  # 이미지도 가능
```
```Docker
docker container prune  # 정지된 컨테이너 삭제

docker image prune  # 실행중인 컨테이너 image 외의 이미지 삭제

docker system prune  # 정지된 컨테이너, 실행중인 컨테이너 image 외의 이미지, 볼륨, 네트워크 삭제
```