# 시작?
Dockerfile 을 생성해서 container 에 필요한 파일들을 셋팅한다
`docker build` 명령을 사용해서 이미지를 빌드한다
build 한 image 를 docker hub 에 push 한다

# dockerfile 작성
고오급 개발자는 바로 Dockerfile을 만들 수도 있겠지만, 일반 개발자들은 일단 리눅스 서버에서 테스트로 설치해보고 안 되면 될 때까지 삽질하면서 
최적의 과정을 Dockerfile로 작성해야 합니다. 우리는 초보니까 Ruby 웹 애플리케이션을 ubuntu에 배포하는 과정을 먼저 살펴보겠습니다.

# Docker build
```bash
docker build -t app .
docker build -t snowdeer/hello-nodejs:v1 .
```

# docker images
이미지가 잘 생성되었는지 확인
```bash
docker images
```

# docker run




# ref
[docker-guide](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)