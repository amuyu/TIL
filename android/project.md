# android version
compileSdkVersion, buildToolsVersion, minSdkVersion, targetSdkVersion

# folder tree
data - repository - remote, local, api
화면 - adapter, domain(usecase, model), presenter, activity
utils, application, injection, Base class
## todoapp 참고
- 기능(화면)별로 분류, 폴더 안에서 도메인, usecase, ui 를 구분한다
- Base클래스는 밖에
task - activity, domain, task, model
data.source - local, remote, repository
BaseView, BasePresenter, App, UseCase, UseCaseHandler, UseCaseScheduler

# injection

# architercutre
mvvm, mvp

# db
room, realm

# library
logger, rxjava, dagger

# language
java, kotlin, retrolambda

# stream
데이터의 흐름

# UI
databinding, butterknife


# 개발 Flow
프로젝트 생성
base 클래스 작성
비즈니스 모델 작성
layout 작성
Activity/Fragment 작성
contract(view/presenter) 작성
repository 작성 - local, remote
usecase 작성

## 세부 구현
UI(화면) > Activity > event 처리를 위해서.. > contract(view/presenter) > 데이터 연동을 위해서.. > repository 작성 > Usecase 작성
Domain > Repository 의 interface DataSource 작성 > RemoteRepository 작성 > UseCase 작성
UI 와 Domain을 합쳐야 한다.
기능들을 잘게 잘게 쪼갠다.


## 필요한거
UI 디자인, 화면에서 사용하는 기능 분류, 스토리 보드, 서버 명세서

## UI 기능 추가
Contract(view/presenter) 추가
fragment 추가
domain 추가
인터페이스를 사용하면 실제 코드가 구현되어 있지 않아도 추가가 쉽다.
