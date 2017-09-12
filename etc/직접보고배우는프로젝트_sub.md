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




===========================
# Coordinator Example
## SimpleCoordinatorActivity
심플한 CoordinatorLayout 이다.
Coordinator, CollapsingToolbarLayout 사용,
스크롤 이벤트에 따라 Appbar 의 헤더 부분의 크기가 변한다.

## IOActivityExample
Toobar의 위치를 Appbar의 child가 아니라 anchor 속성을 사용하여 지정하였다.


## MaterialUpConceptActivity









===========================
# AdapterDelegate
## source
[AdapterDelegates](https://github.com/sockeqwe/AdapterDelegates)
## summary
하나의 리스트에 여러 형태의 item view 구성을 도와주는 라이브러리
RecyclerView 의 경우, 여러 형태의 view를 구성하기 위해
viewtype, viewholder, createviewholer, bindviewholder 를 수정하는데 이에 대한 코드를 Delegate 코드로 분리함

## 정리
### AdapterDelegate
This delegate provide method to hook in this delegate to RecyclerView.Adapter lifecycle.
abstract class
### AdatperDelegatesManager
This class is the element that ties {@link RecyclerView.Adapter} together with {@link AdapterDelegate}.
AdapterDelegate 관리
#### GenericType
Data Source Type을 Generic으로 사용
#### SparseArrayCompat
AdapterDelegate 리스트를 SparseArrayCompat 을 사용
Lint 에서 HashMap 보다는 SparseArray 를 추천함 (안드로이드에서는 performance가 더 높다고 한다)
#### viewtype
delegates 의 position 이 하나의 view type 이 된다. (delegate == viewtype)

## 배울 점
### Exception 처리
에러에 대해서 throw Exception 처리
### 주석
클래스, method 에 상세하기 주석


===========================
# android-rxjava [study](https://github.com/gdgand/android-rxjava)
rxjava study 프로젝트
## Queue app module
PublishSubject를 이용한 queue 사용
### Queue2Activity
### onBackPressureBuffer
PublishSubject는 크기의 한계가 있기 때문에 onBackPressureBuffer 를 같이 쓰는게 좋음
사용하지 않았을 경우 rx.exceptions.MissingBackpressureException 발생함, 처리하는 속도가 따라가지 못해 발생하는 현상
#### Command
Command 인터페이스 사용해서 PublishSubject에서는 BottomCommand와 TopCommand 상관 없이
Command 인터페이스의 call 메소드를 호출함
### FasterListActivity
observable을 사용해서 scroll이 빨라진다
RecyclerView의 Adapter에서 Observable 안에서 백그라운드 쓰레드로 처리함으로써 RecyclerView의 Frame 속도를 개선할 수 있음.
bindViewHodler 에서 작업이 오래 걸리는 경우에, 예제 소스에서는 sleep 이 걸려있음.
sleep 이 백그라운드에서 돌면서 scroll 이 빨라짐,, sleep 빼면 똑같음

## Hot and Cold app module
Hot and Cold observable 에 대해서 살펴보는 프로젝트
### hot and cold 개념
#### Hot - multicast
subscriber의 구독과 상관없이 아이템을 발행(emit), connectobservable
연산 작업이 모든 subscriber에 공유
상태값의 유지가 필요한 작업의 경우 사용
#### cold Observable
subscriber가 구독하면 아이템을 발행 (subscriber 마다), 일반적인 observable
각각의 subscriber 마다 연산작업이 필요
상태값의 유지가 필요하지 않은 경우 사용
### HotAndColdActivity
#### connect() 없이 hotobservable을 사용하는 방법
publicshsubject의 refcount() 사용하면 된다
### ReplayingShare
구독 직전의 emit된 item도 받을 수 있다
### CombineLatest
여러 observable을 모아서 처리
### bindToLifecycle
bindToLifeCycle()은 기본적으로 subscribe 한 life cycle에 따라서,
onCreate()는 onDestroy(),
onStart()는 onStop(),
onResume()는 onPause(),
onStop()은 onDestroy()
에서 unsubscribe를 하게 됩니다
### compose


## 배울 점
### hot 과 cold observable 어떤 경우에 사용하는가?
하나의 stream에 하나의 subscriber 인 경우 - observable
하나의 stream에 여러 subscriber 인 경우 - Observable.publish().refCount()
하나의 stream에 여러 subscriber 이고 이전에 emit된 item 이 필요한 경우 - Observable.replay().refCount()
하나의 stream에 여러 subscriber 이고 최근 emit된 item이 필요한 경우 - Observable.compose(ReplayingShare.instance())



======================================
# logger
log 저장하는 라이브러리 프로젝트
dependencies가 없다(dependencies 어떻게 하는지 보려고 했는데..)
## 클래스 호출
Logger > LoggerPrinter > LogAdapter > FormatStrategy
Logger에서는 LoggerPrinter를 사용해서 log message print
LoggerPrinter에서는 List<LogAdapater> 에 log 메시지 전달
LogAdapter에서 FormatStrategy 설정에 따라 log 메시지 print
### Logger 클래스
사용자가 호출하는 인터페이스, 호출하기 쉽도록 static 메소드/변수 사용
### LoggerPrinter
사용자가 호출하는 메소드를 LogAdapter에 전달할 수 있도록 정리
(여러 종류의 LogAdapter 사용 가능)
### LogAdapter
출력 장소에 따라 클래스를 분리 AndroidLogAdapter, DiskLogAdapter
실제 출력은 FormatStrategy 에서 이루어지지만 사용자가 용도에 따라 사용할 있도록 인터페이스 용도로 사용
인터페이스 클래스보다는 추상 클래스로 사용이 더 나은 듯....
#### AndroidLogAdapter
Logcat 에 출력되는 메시지 PrettyFormatStrategy 클래스에 메시지 전달
### FormatStrategy
실제 메시지 출력 담당
### PrettyFormatStrategy
log를 예쁘게 출력

======================================
# Timber
log 저장하는 라이브러리 프로젝트
## tree(abstract)
로그 호출 인터페이스
### ThreadLocal
쓰레드 단위로 로컬 변수를 할당한다,
쓰레드 영역에 변수를 설정한다.
쓰레드 기준으로 동작하는 기능 구현시 , 유용하게 사용가능하다
Tree에서 ThreadLocal을 사용한 이유는 뭘까? 다른 thread 에서 tag를 채가지 않도록 하기 위해서
tag 입력은 기존의 입력처럼 Logger(tag, message) 형태로 사용하도록 하는게 나을듯
## TREE_OF_SOULS
tree 호출(리스트) - adapter 역할
## DebugTree
실제 동작하는 tree
## Test
Robolectric, festassert 사용
shadowlog, LogAssert

## 배울점
### 멀티쓰레딩
volatile, synchronized, threadlocal
### list
unmodifiableList


======================================
# android component architecture - BasicSample
MVVM
## ProduceListFragment
Databinding, LifecycleFragment, AndroidViewModel 사용
### data 갱신
viewmodel 에 mObservableProducts 를 observe 해서 변화가 발생하면 adapter list 갱신
### list click 이벤트
callback을 adpater에 넘겨주고 binding 을 이용해 xml 로 callback을 넘겨줌
callback > adapter > xml
### databinding inflate
'DataBindingUtil.inflate' 를 사용하여 binding 생성
ViewDataBinding.bind() 와는 큰 차이는 없음
```java
DataBindingUtil.inflate(inflater, R.layout.list_fragment, container, false)
```
### Recyclerview 와 binding
ViewHolder에 멤버변수로 binding 사용
```
static class ProductViewHolder extends RecyclerView.ViewHolder {

  final ProductItemBinding binding;

  public ProductViewHolder(ProductItemBinding binding) {
      super(binding.getRoot());
      this.binding = binding;
  }
}
```
## ProduceListViewModel
Data 에 접근하는 클래스로 DatabaseCreator 사용

## DatabaseCreator
Room 사용

## LifecycleFragment
## LiveData


===========================
# android component architecture - GithubBrowserSample
Dagger.android 를 사용함
## Application
### HasActivityInjector
Application 에서 사용하며, 이렇게 셋팅해놓으면 Activity 단위에서
`DaggerComponent.builder()..` 의 호출없이 좀 더 쉽게 inject 호출할 수 있음
inject 호출 시, 사용
Application 에 interface 구현
구현 후,
```java
DaggerYourApplicationComponent.create()
        .inject(this);
```
이렇게 하면 Activity 에서 이렇게 호출하면 됨
```java
public class YourActivity extends Activity {
  public void onCreate(Bundle savedInstanceState) {
    AndroidInjection.inject(this);
    super.onCreate(savedInstanceState);
  }
}
```
가이드에 나오는 내용..
[dagger.android](https://google.github.io/dagger//android.html)

### @Component.Builder
Component.builder 에 대한 인터페이스 추가

### ViewModelModule
AppMoule에 viewmodelmoudle 이 들어간다

### AppInjector
DaggerAppComponent.builder()를 호출해서 inject 해주는 클래스

## Single Activity
### NavigationController 사용
Fragment에서 다른 fragment 로 이동할 때 사용

## SearchFragment
### lifecycle
onCreateView 에서 view를 생성하고,
onActivityCreated 에서 view 초기화 등, 셋팅작업
### AutoClearedValue
A value holder that automatically clears the reference if the Fragment's view is destroyed.
### data 갱신
searchviewmodel.results.observe 해서 갱신되면
DataBoundListAdpater의 replace를 호출함


## GithubViewModelFactory
ViewModelProvider.Factory 를 구현

## SearchViewModel

## DataBoundListAdapter
Data Binding & DiffUtil 사용한 Adapter
replace 사용, viewModel에서 받은 list와 adapter의 list를 비교해서 데이터를 갱신함 - diffutil 사용


## etc
### manifest 분리
debug 에 대한 별도 manifest 사용





===========================
# MVVM-with-AAC [MVVM-with-AAC](https://github.com/ZeroBrain/MVVM-with-AAC)
This source is using Dagger, Databinding, Room, ViewModel, LiveData and MVVM based.
Stetho 를 사용함
kotlin extension 사용

## LottieApplication
LottieApplication 생성 시, LottieApplicationComponent를 생성하고
LottieApplicationComponent를 Activity 에서 가져다가 쓸 수 있게 함
LottieListActivity 에서 appComponent를 주입함
## LottieApplicationDagger.kt - LottieApplicationComponent
App에서 공통적으로 사용하는 모듈들을 주입함 (okhttp, database 등)
LottieApplicationComponent 에서 lottieListComponent(@Subcomponent) 도 추가 주입할 수 있도록 함
DaggerLottieListComponent.builder()를 호출하지 않아도 됨
LottieApplicationComponent 와 LottieApplicationModule 를 정의
java에서는 따로따로 생성했던 것 같은데, kotlin에서는 함께 작성?
filepath method 에 object 대입?
### LottieApiModule
okHttpClient, retrofit 생성
### LottieDatabaseModule
database 생성(room)
### LottieDaoModule
LottieDao 생성


## LottieDagger.kt - LottieListComponent
subcomponent, LottieAppComponent 에 이어 호출
LottieListComponent를 생성하기 위한 Dagger 클래스를 만들지 않아도 된다.
### LottieListModule
LottieModel 생성, inject 클래스
LottieListViewModel 생성
ViewModelProvider 생성 - ViewModelProvider.Factory 를 사용해서 ViewModel 생성함


## LottieListActivity
Application 클래스의 appcomponent를 호출해 subcomponent와 결합 후, inject
lazy 사용
### addObserver?
LifecycleRegistry의 addObserver 를 호출
```kotlin
lifecycle.addObserver(lottieListViewModel)
```
viewModel 에서 lifecycle event 에 따라 직접 컨트롤
```kotlin
@OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun onInit() {
        lottieModel.refresh()
}

@OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
fun onDestroy() {
    lottiesDisposable.takeIf { !it.isDisposed }?.dispose()
}
```
### act_lottie_list.xml
EditText 에서 onTextChanged 속성에 method 호출을 사용 방법
```xml
<EditText
  android:onTextChanged="@{(text, _1, _2, _3)-> viewmodel.onSearch(text)}"/>
```
RecyclerView 에 items 적용 방법 app:lottieitems 사용 recyclerview 에 list 셋팅
binding 으로 알아서 adapter와 연결해준다
```xml
<android.support.v7.widget.RecyclerView
    app:lottieitems="@{viewmodel.lotties}" />
```


## LottieListAdapter
onCreateViewHolder 에서 그리드 뷰의 width 를 계산해서 셋팅함
```kotlin
return LottieViewHolder(lottieItemBinding.apply {
            val columnWidth = root.resources.displayMetrics.let { Math.min(it.widthPixels, it.heightPixels) / columnCount }
            root.layoutParams = root.layoutParams.apply {
                width = columnWidth
                height = columnWidth
            }
        })
```


## lottieListViewModel
BehaviorProcessor subject 사용
FlowableForLifecycle.FlowableForLifecycle 사용 > custom function
subscribeIgnoreError 사용 > Extension Function 사용
onSearch 사용
@OnLifecycleEvent 사용, onCreate 와 onDestroy에 어떤 작업을 할 지 구현함
### rxJava
distinctUntilChanged 사용, 데이터 변화가 생기면 emit
sample 사용, 일정 시간 안에 발생한 최근 이벤트를 emit


## LottieModel
lottiedao, lottieApi, downloadApi, filePath 사용해서
refresh(api), getData(db), search(db) 동작 수행
LottieModel 에서 리스트를 가지고 있는게 아니라 Flowable<List<Lottie>>, LiveData<List<Lottie>> return type 으로 사용


## RxUtil.kt
boilerplate code

## 배울점
### subcomponent 사용
ApplicationComponent에 이어 LottieListComponent라는 subcomponent 를 사용
appcomponent에서 apimodule, databasemodule 을 생성함
### scope 사용
ApplicationScope를 사용
```kotlin
@Scope
@Retention(AnnotationRetention.RUNTIME)
annotation class ApplicationScope
```
@retention 은 컴파일러 옵션을 표시하는 annotation 임


===========================
# BasicRxJavaSample [github](https://github.com/googlesamples/android-architecture-components/tree/master/BasicRxJavaSample)
This is an api sample to showcase how to implements observable queries in Room, with RxJava's Flowable object.
The sample app shows an editable user name, stored in database
## 살펴보기
### Injection
This injects UserDataSource and ViewModelFactory.
UserViewModel을 주입하기 위함
#### UserDataSource
Interface for accessing user data(db)
There are getUser, insertOrUpdateUser, deleteAllUsers methods
##### LocalUserDataSource
Using the Room database as a data source
### UserActivity
CompositeDisposable 를 사용해서 view를 갱신하는 stream 이벤트를 관리한다.
### ViewModelFactory
UserViewModel 을 생성하기 위한 Factory
UserViewModel 은 생성할 때, UserDataSource 를 전달인자로 필요로 하기 때문에 Factory 클래스 추가가 필요함
### UserViewModel
ViewModel 클래스 사용, RxJava를 사용해서 db 조회 후, view 를 갱신함  
Completable 사용, 결과만 전송하는 이벤트
getUserName 을 보면 Flowable<String>을 리턴하는데 LiveData 가 아니어도 데이터가 변경되면 emit 한다
이상하네 LiveData 를 추가하면 되는데 안하면 안되네...


===========================
# todoapp
[todoapp](https://github.com/googlesamples/android-architecture.git)
## todo-mvp-clean
This sample stands on the principles of Clean Architecture.
### Injection
Injection클래스를 사용해서 필요한 클래스를 주입한다. Dagger로 대체 가능하다
