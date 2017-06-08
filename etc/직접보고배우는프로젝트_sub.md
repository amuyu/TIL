# delicious
## source
[mvp 패턴 사용](https://github.com/delicious-mvp/delicious)
## summary
daum api 사용, 검색 기능, webview 사용 검색 결과 호출
## 정리
### BaseActivity, BaseFragment
Activity, Fragment 사용시 BaseActivity, BaseFragment 상속받아 사용
#### BaseActivity
Toolbar 셋팅
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

## 배울 점
인터페이스 활용 : 클래스에서 서로 호출하는 부분은 인터페이스를 사용
adapterdelegate 사용
## 개선
dataviewmodel 적용



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
# android component architecture
## ProduceListFragment
Databinding, LifecycleFragment, ProductListViewModel,
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
## LifecycleFragment
## LiveData
