# delicious
## summary
daum api 사용, 검색 기능, webview 사용 검색 결과 호출

## 배울 점
gradle.properties를 사용(daum_api_key 관리)
dependencies의 version 을 별도로 관리(project.gradle의 ext 에 입력)
BuildConfigField 사용
인터페이스 활용 : 클래스에서 서로 호출하는 부분은 인터페이스를 사용
adapterdelegate 사용

## daum_api_key
project.hasProperty 를 사용해서 daum_api_key 를 입력했는지 확인하고 입력이 안되어 있으면 빌드되지 않도록 구현
```
// build gradle
buildConfigField "String", "DAUM_API_KEY", "\"${rootProject.getProperties().get("daum.apikey")}\""
// properties
daum.apikey=xxx
```

## source
[mvp 패턴 사용](https://github.com/delicious-mvp/delicious)
### Application 클래스
applicationHandler를 static으로 두고 사용함, Global 클래스에서 사용하고 다른 클래스에서 Global 클래스를 통해 Handler를 사용함

### BaseActivity
Activity 의 boilerplate 코드 담당
Toolbar 셋팅, Butterknife.bind 호출

### MainActivity
#### layout
include 를 사용해서 부분별(toolbar, main, naviview)로 나눠서 관리함
DrawerLayout, NavigationView 사용
#### ui
Snackbar 메시지, DrawerToggle, naviationview 사용
#### Network
rxjava, okhttp 를 사용해서 네트워크 통신함

### MainActivity
#### layout
CoordinatorLayout,

#### BaseFragment
presenter를 Generic Type으로 사용
##### 공통으로 사용하는 것 들
Presenter, View, BaseActivity, ProgressDialog

### MVP 공통으로 사용하는 것들
Activity 에서 presenter와 fragment(view)를 생성한다.
이를 위해 공통적으로 사용하는 것들에 대해 추상클래스와 인터페이스로 정의하여 사용
실제로 presenter를 사용하는 클래스는 fragment(view) (반대로 view를 사용하는 클래스는 presenter)
그런데 하나의 activity 에서 여러개의 fragment 를 사용하는 경우는? fragment 에서 생성해서 쓰는게 편할 듯
### BasePresenter
presenter 기본 interface
### BaseView
presenter를 Generic Type으로 사용하는 view 기본 interface
### AbstractPresenter
view 를 Generic Type으로 사용하는 추상 클래스
생성 시, view에 pesenter를 셋팅
view return

### SearchActivity
#### RecyclerView, Adapter
adpater 인터페이스를 데이터 관리하는 부분과 이벤트 처리하는 부분으로 나누어 처리 (SearchDataModel, SearchAdatperView)
SearchAdapterView 인터페이스의 실제 호출은 fragment(view)임
#### PublishSubject
publishsubject를 사용해서 리스트 호출
#### AdapterDelegate
AdapterDelegate를 사용

### 폴더 관리
#### ui
화면 별로 나누고, 화면안에서 presenter, adapter(model, view) 기능별로 나눔
ui - search - adapterdelegate - model
                              - view
            - presenter

## 개선
dataviewmodel 적용
