# docker-compose ??
multi-container Docker applications 에 대한 정의 실행


# 필요한 파일
Dockerfile
requirements.txt
docker-compose.yml


# 실행
sudo docker-compose run web django-admin.py startproject composeexample .
프로젝트 수정 후,,
docker-compose up

# 도커 이미지 빌드 및 실행
```sh
docker build -t django-sample .
docker run -it --rm -p 8000:8000 django-sample ./manage.py runserver 0:8000
```

# 도커 명령 구조
```sh
docker run [옵션] 이미지이름[:태그] [명령어] [전달인자]
```

# 옵션 설명
- it : 컨테이너의 표준 입력과 로컬 컴퓨터의 키보드 입력을 연결
- rm : 컨테이너를 종료할 때 컨테이너 삭제
- p : 컨테이너 외부와 내부를 연결할 포트
- d : detached mode 백그라운드 모드
- v : 호스트와 컨티에너의 디렉토리를 연결
- e : 컨테이너 내에서 사용할 환경변수 설정
- link : 컨테이너 연결


# 참고
[docker로 django개발 환경 만들기](http://nberserk.github.io/default/2016/02/23/docker-django.html)
[가장 빨리 만나는 Docker 18장 Docker로 Django 애플리케이션 구축하기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter18)
[Quickstart: Compose and Django](https://docs.docker.com/compose/django/)
  - docker documents 로 tutorial
[Django Development With Docker Compose and Machine](https://realpython.com/blog/python/django-development-with-docker-compose-and-machine/)
[Docker(Compose) 활용법 - 개발 환경 구성하기](http://raccoonyy.github.io/docker-usages-for-dev-environment-setup/)
