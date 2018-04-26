# 장고 개발의 기본 사항
장고 웹프로그래밍의 특징은 쉽고 빠르다는 것이다.
웹 프로그래밍에서 공통적으로 필요한 기능들을 미리 만들어 단축함수, 제네릭 뷰 등으로 제공하고
서드 파티에 의한 외부 라이브러리가 풍부해 개발이 빠르다.

## MTV 개발 방식
웹 프로그래밍 영역을 Model, Template, View 로 구분해서 개발을 진행한다.
Model : 테이블 정의
Template : 화면의 모습을 정의
View : 앱의 제어 흐름 및 처리 로직을 정의

## MTV 코딩 순서
특별하게 정해진 순서는 없지만 보통 이런 식으로 많이 한다.
프로젝트 뼈대 만들기 > 모델 코딩 > URLConf 코딩 > 뷰 코딩 > 템플릿 코딩

## settings.py 주요 사항
settings 는 프로젝트 설정 파일이다.
- 데이터 베이스 설정
- 템플릿 항목 설정
- 정적 파일 항목 설정
- 애플리케이션 등록
- 타임존 지정

## models.py 주요 사항
models 는 테이블을 정의하는 파일이다. 장고는 데이터베이스 처리에 ORM 기법을 사용한다.
테이블을 클래스로 매핑해서 테이블에 대한 CURD 기능을 클래스 객체에 대해 수행하면, 장고가 내부적으로 데이터베이스에 반영해 준다.

테이블의 신규 생성, 테이블의 정의 변경 등 models.py 에서 데이터베이스 변경 사항이 발생하면
데이터베이스에 실제로 반영해주는 작업을 해야 한다. 이를 위해 마이그레이션이라는 개념을 도입했다.
migrations 디렉토리 하위에 마이그레이션에 대한 정보를 작성하고 makemigrations 및 migrate 명령을 사용해
마이그레이션 한다.

## URLconf 주요 사항
URLconf 는 URL 과 뷰를 매핑해주는 urls.py 파일을 말한다.
필자의 경우 프로젝트 URL, 앱 URL 2계층으로 나눠서 코딩하는 방식을 사용한다.

## views.py 주요 사항
뷰 로직을 코딩하는 가장 중요한 파일이다.
장고에서는 뷰 로직을 함수로 코딩할 것인지 클래스로 코딩할 것인지에 따라 함수형 뷰와 클래스형 뷰로 구분한다.
클래스 형 뷰를 사용하면 제너릭 뷰를 사용할 수 있고 재활용 및 확장성 측면에서 유리할 수 있다.

## templates 주요 사항
웹 화면 페이지 별로 템플릿 파일이 하나씩 필요하므로 웹 프로그램 개발 시 여러 개의
템플릿 파일을 작성하게 되고, 이런 템플릿 파일들을 한 곳에 모아두기 위한 템플릿 디렉토리가 필요하다.

템플릿 디렉토리는 프로젝트 템플릿 디렉토리와 앱 템플릿 디렉토리로 구분해서 사용한다.
프로젝트 템플릿 디렉토리는 TEMPLATES 설정의 DIRS 항목에 지정된 디레토리이다.
앱 디렉토리는 각 애플리케이션 디렉토리마다 존재하는 templates 디렉토리이다.

장고에서 템플릿 파일들을 찾는 순서는 `프로젝트 템플릿 > INSTALED_APPS 설정 항목`에 등록된 순서대로 검색한다.

## Admin 사이트
Admin 사이트는 테이블의 내용을 열람하고 수정하는 기능을 제공한다.
Admin 사이트에 원하는 테이블을 등록하기 위해서는 admin.py 파일에 작업하면 된다.



# 실전 프로그램 개발 - Bookmark 앱
자주 방문하는 사이트를 등록해두는 북마크 앱을 만들어보자

## 애플리케이션 설계하기
사용자의 눈에 보이는 화면 UI, 화면에 접속하기 위한 URL, 서버에서 필요한 테이블 및 처리 로직 등을
설계한다.

## 화면 UI 설계
북마크 타이틀 리스트와 북마크 상세 화면으로 나눈다.

## 테이블 설계
models.py 에 코딩한다. Bookmark 테이블 하나만 필요하다.
id, title, url

## 로직 설계
웹 프로그래밍에서는 URL을 받아서 최종 HTML 템플릿 파일을 만드는 과정이 하나의 로직이 된다.
여기서는 간단하게 URL - 뷰 - 템플릿 간의 처리 흐름을 표시한다.

## URL 설계
URL 설계 내용은 URLconf 코딩에 반영되고, urls.py 파일에 코딩한다.
URL 패턴, 뷰 이름, 템플릿 파일 이름 및 뷰에서 어떤 제네릭 뷰를 사용할 것인지 등을 결정한다.
/bookmark/      BookmarkLV      bookmark_list.html
/bookmark/99/*  BookmarkDV      bookmark_detail.html

## 작업 코딩 순서
모델, URLconf, 뷰, 템플릿 순서로 진행한다.
|작업순서|관련 명령 파일|필요한 작업내용|
|---|---|---|
|뼈대 만들기|strtproject|mysite 프로젝트 생성|
|         |settingis.py|프로젝트 설정 항목 변경|
|         |migrate|User/Group테이블생성|
|         |createsuperuser|프로젝트 관리자인 슈퍼유저를 만듦|
|         |startapp|북마크 앱 생성|
|         |settings.py|북마크 앱 등록|

|모델 코딩하기|models.py|모델 정의|
|         |admin.py|Admin사이트에 모델 등록|
|         |makemigrations|모델을 데이터베이스에 반영|
|         |migrate||

|URLconf 코딩하기|urls.py|URL 정의|
|뷰 코딩하기|views.py|뷰 로직 작성|
|템플릿 코딩하기|templates디렉토리|템플릿 파일 작성|

## 개발 코딩하기 - 뼈대
프로젝트, 앱 생성
```sh
django-admin startproject mysite  // 프로젝트 생성
python manage.py migrate  // 기본 테이블 생성 (사용자, 사용자의 권한 그룹 테이블)
python manage.py createsuperuser  // 관리자 생성
python manage.py startapp bookmark  // 북마크 앱 생성
```
settings.py 에 생성한 bookmark 앱을 등록한다.
settings.py 의 INSTALLED_APPS 에 'bookmark.apps.BookmarkConfig' 로 등록한다.

## 개발 코딩하기 - 모델


## 장고의 패키지 검색
https://pypi.python.org
https://www.djangopackages.com
