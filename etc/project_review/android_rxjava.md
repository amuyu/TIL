# android-rxjava [study](https://github.com/gdgand/android-rxjava)
rxjava에 대해서 study 할 수 있는 프로젝트
## Queue app module
PublishSubject를 이용한 queue 사용

### Queue1Activity
Push 메세지를 딜레이 없이 20개 수신해서 Notification 알림을 띄우는 앱
Subject를 사용한 것과 하지 않은 것을 비교한다.
#### Subject가 없는 경우,
이벤트가 발생한 대로 Notificaiton 이 발생하기 때문에 20개의 Noti가 순식간에 띄워진다.
#### Subject가 있는 경우,
`throttleLast` operator를 사용하기 때문에 700ms 동안 발생한 이벤트 중, 마지막 이벤트만 호출한다.
#### 결론?
발생한 이벤트를 다른 형태로 변형해서 사용하기 용이하다.

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
