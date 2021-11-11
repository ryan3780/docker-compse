# docker-compose로 협업 환경을 만들어 보자
### 1. https://hub.docker.com/ 에서 ubuntu 이미지를 pull  
### 2. pull 받은 이미지에 장고 설치하고, 새로운 이미지로 만들기

```
pull 받은 이미지 실행

docker run -it ubuntu:latest /bin/bash

위 명령어로 해당 이미지 안에서 작업을 할 수 있다.
예시는 우분투에 장고를 설치하고 `commit` 명령어로 새로운 이미지를 만들었다.

docker commit 컨테이너이름 새로운이미지이름

```

### 3. 원하는 디렉토리 구조를 만들고, 도커 파일 만들기
```
FROM ele3780/ubuntu-django #새롭게 만든 이미지
RUN mkdir -p test #테스트 라는 디렉토리를 만들라는 명령어
WORKDIR /project/server # 해당 경로에서 작업해라
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"] 
#명령어를 실행해라

* CMD와 RUN은 조금 다릅니다.
* 참고 URL : https://williamjeong2.github.io/blog/10-docker-run-vs-cmd-vs-entryporint/
```
### 4. docker-compose.yml을 만들기
```
version: "3.7"

services:
  backend:
    build: Docker-Files/BE/
    volumes:
      - ./server:/project/server
    ports:
      - 8000:8000

* 여기는 한번 찾아 보세요.
```
## 호스트와 컨테이너의 연동 문제가 발생한다면  
### python3: can't open file 'manage.py'
### 만약 이런 문제라면 호스트(로컬)에 연동하려는 폴더에 가서 django-admin startproject 여기서는 server 해서 파일과 디렉토리의 구조가 같게 해주면 연동이 된다.