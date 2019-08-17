# go 프로젝트 layout
Standard Go Project Layout
[project-layout](https://github.com/golang-standards/project-layout)
공식 표준은 아니지만 일반적인 프로젝트 레이아웃에 대한 설명이다.

## cmd
Main applications for this project.
응용 프로그램 디렉터리 이름은 원하는 실행 파일과 이름이 일치해야 한다.
해당 디렉터리에 너무 많은 코드를 넣어서는 안된다.
중요한 코드는 /internal 이나 /pkg 에 작성하드

## internal
개인 응용 프로그램 및 라이브러리 코드, 다른 사람이 사용하기를 원하지 않는 코드

## pkg
외부 응용 프로그램에서 사용하는 재사용하는 코드

## vendor
Application dependencies

## api
OpenAPI / Swagger 사양, JSON 스키마 파일, 프로토콜 정의 파일

## web
웹 응용 프로그램

## configs
구성 파일 템플릿 또는 기본 구성

## init
시스템 초기화 (systemd, upstart, sysv) 및 프로세스 관리자 / 감독자 (runit, supervisord) 구성.

## scripts
다양한 빌드, 설치, 분석 등을 수행하는 스크립트

## build
Packaging and Continuous Integration.

## deployments
배포 구성 및 템플릿 
