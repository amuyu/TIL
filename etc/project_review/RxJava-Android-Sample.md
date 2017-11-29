===========================
# RxJava-Android-Sample
Learning RxJava for Android by example
[GitHub](https://github.com/kaushikgopal/RxJava-Android-Samples)


## build.gradle
kotlin, lifecycle, butterknife, retrofit, volley, rxjava, rxbinding 사용
## com.f2prateek.javafmt
## com.jakewharton.rxrelay2
Relays are RxJava types which are both an Observable and a Consumer.
relaySubject 같은 역할을 한다. 구독하기 시작할 때, 그 사이에 배출되는 항목들을 잃어버릴 수 있기 때문에 사용한다.
## com.jakewharton.rx:replaying-share-kotlin
An RxJava 2 transformer which combines replay(1), publish(), and refCount() operators.
[github](com.f2prateek.javafmt)
[RxJava —RxReplayingShare, Emit only Once](https://medium.com/@p.tournaris/rxjava-rxreplayingshare-emit-only-once-b19acd61b469)
## 요건 뭐지?
pickFirst : 하나 이상의 path가 있을 때, 첫 번째로 찾는 것을 선택한다.
```groovy
packagingOptions {
        pickFirst  'META-INF/rxjava.properties'
    }
```


## Application
MultiDexApplication 을 사용
LeakCanary, Volley, Timber 초기화

## BG WORK (SCHEDULERS & CONCURRENCY) - ConcurrencyWithSchedulersDemoFragment
subscribeOn과 observerOn 을 사용해 비동기 작업을 하도록 구현
### DisposableObserver
An abstract {@link Observer} that allows asynchronous cancellation by implementing Disposable.
### Log
Log를 남기는 방식을 보면 MainThread인 경우에만 UI를 갱신하고 그렇지 않은 경우에는 리스트만 추가하고 있다.
```java
private void _log(String logMsg) {

    if (_isCurrentlyOnMainThread()) {
      _logs.add(0, logMsg + " (main thread) ");
      _adapter.clear();
      _adapter.addAll(_logs);
    } else {
      _logs.add(0, logMsg + " (NOT main thread) ");

      // You can only do below stuff on main thread.
      new Handler(Looper.getMainLooper())
          .post(
              () -> {
                _adapter.clear();
                _adapter.addAll(_logs);
              });
    }
}
```
### Looper.myLooper()
현재 Thread 에서 사용하는 Looper 를 호출한다.
UI 작업을 하기 위해서는 `Looper.getMainLooper()` 를 사용해야 한다.
### 배울 것
** Observer와 Observable를 별도 함수에서 구현 **
** Subscribe 호출 시, just와 map 을 사용해서 함수 호출하도록 구현 **
** observable관리는 CompositeDisposable를 사용한다. **



## BufferDemoFragment
`buffer` operator를 사용하도록 구현
buffer operator 는 Observable에 의해 방출된 항목을 주기적으로 수집해서
주기동안 발생한 항목들을 한꺼번에 방출합니다
주기동안 항목이 방출되지 않으면 observer 에게 빈 리스트를 전닯합니다.
### SubscribeWith
Rxjava2 에서는 subscribe 에서 subscription 을 리턴하지 않습니다.
subscribewith를 사용해서 Disposable 을 리턴받아 사용해야 합니다.
### 배울 것
** RxView 사용 **
RxView 를 사용해서 버튼 이벤트에 대해 Observable 형태로 받을 수 있다.
`ViewClickObservable` : click 이벤트 리스너를 구현해서 이벤트가 발생할 때, onNext를 실행한다.


## DebounceSearchEmitterFragment
`debouce` operator를 사용하도록 구현
debounce operator 는 특정 시간대 동안 다른 항목을 방출하지 않고
시간이 경과한 경우에만 Observable에서 항목을 방출합니다.
시간 동안 새로운 항목이 발생하면 다시 특정 시간을 기다립니다.
### RxTextView.textChageEvents
EditText의 Text change Event를 받을 수 있다.


## RetrofitFragment
Retrfit 과 Rxjava 를 사용해 api 호출 구현
### Observable.zip 사용
2개의 observable 항목을 결합해서 하나의 항목으로 방출한다.
2개의 항목이 모두 발생해야 방출한다.
### 배울 것
** OkHttpClient 에 Header 추가하기 **


## DoubleBindingTextViewFragment
PublishProcessor 를 사용해 textChanged 이벤트가 발생했을 때,
두개의 EditText 로부터 텍스트를 입력받아 observer 에게 항목 배출
### 배울 것
** Butterknife.Unbinder 사용 **


## PollingFragment
특정 시간 간격마다 Polling하도록 구현
### SIMPLE POLLING
`interval` operator 를 사용
특정 시간 간격마다 증가하는 정수를 방출한다.
`take` operator 를 사용
순서대로 정의한 항목 개수만큼 방출한다.
### INCRESAINGLY DELAYED POLLING
`repeatWhen` operator 를 사용
`Function<Flowable<Object>, Publisher<Long>>` 를 구현해서
항목을 반복해서 발생시키고 항목이 발생했을 때, timer operator로 delay를 준다.
### 배울 것
** TimeUnit 사용 **
TimeUnit.MILLISECONDS.toSeconds 이런 방법으로 단위를 변경할 수 있음


## RxBusDemoFragment
PublishRelay 를 사용해 RxBus 를 구현
RxBus 로 TabEvent 를 전달받고 전달받은 TabEvent 를 multi 로 observe 한다.
하나는 TabEvent를 표시하고, 하나는 지속되는 TabEvent Count 표시한다.
### flowable.Publish
ConnectableFlowable 로 Hot Observable 사용
### debounce 와 buffer 조합 사용
debounce 와 buffer 를 조합해서 항목 발생 시점부터 특정 시간동안 발생한 항목들을
모아서 처리한다.


## FormValidationCombineLatestFragment
email, password, number edittext 세 가지의 입력을 받아
세 Observable 를 결합해서 처리한다. 세 가지 Observable 이 발생해야 emit 한다.
### 배울 것
** skip 사용 **
입력하지 않아도 subscribe 호출로 observable 이 발생하기 때문에 세 가지 입력 observable에
skip(1) 을 사용해서 거른다.


## PseudoCacheFragment
retrieve data first from a cache, then a network call
( RxJava를 적용하며 비교적 빠른 디스크 캐시와 비교적 느린 네트워크 호출을 끊김 없이 사용자들에게 가공해서 보여주는 방법,
  여기서는 느린 디스크 캐시를 사용한다. )
`concat`, `concateager`, `merge` operator 사용 구현
`concat` : 첫 번째 observable을 항목들을 모두 배출한 후, 두 번째 observable 항목을 배출한다.
`merge` : 여러 observable 에서 배출되는 항목 중 하나를 배출한다. (배출되는 순서대로 배출한다.)
publish, takeUntil, merge 를 조합해서 안전하게 네트워크 데이터를 보여주도록 구현하는 방법
### takeUntil
다른 observable이 배출 된 후에는 배출되는 항목을 무시한다.
### cache와 네트워크 데이터 호출 구현
다음과 같은 방법으로 캐쉬와 네트워크 호출에 대한 구현을 할 수 잇다.
먼저 cache 데이터를 보여주고 나중에 가져오는 네트워크 데이터를 업데이트 한다.
```java
getFreshNetworkData() //
        .publish(
            network -> //
            Observable.merge(
                    network, //
                    getSlowCachedDiskData().takeUntil(network)))
        .subscribeOn(Schedulers.io()) // we want to add a list item at time of subscription
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe();
```
### 참고
[성격이 급한 존을 위한 끊김 없는 데이터 전송](http://kimjihyok.info/2017/05/29/rxrealcase-1장-성격이-급한-존을-위한-끊김-없는-데이터-전송/)



## TimingDemoFragment
`timer`, `interval`, `delay` operator 를 사용 구현
`timer` : 입력한 delay 후에 emit
`interval` : interval 마다 emit
`delay` : delay 후에



## TimeoutDemoFragment
`timeout` operator 사용 구현
`timeout` : 입력한 시간을 경과하면 timeoutexception 이 발생한다.
timeoutexception 발생 시, 다음 method 호출로 observable 을 대체할 수 있다.
```java
Observable timeout(long timeout, TimeUnit timeUnit, ObservableSource<? extends T> other);
```



## ExponentialBackoffFragment
`retrywhen`과 `delay` operator 사용
에러 발생 시, 여러 번 재시도 하는 루틴과
observable 이 항복을 배출할 때마다 delay 시간을 을 증가하게 하는 루틴 구현



## RotationPersist3Fragment
스마트폰을 rotation 하고 버튼을 터치해도 데이터를 유지한다.
kotlin, viewmodel 사용
viewmodel 에 observable 을 구현
`replay` : ConntectableFlowable 로 변환
`autoConnect` : 입력한 수 만큼의 구독이 발생하면 자동으로 connect를 수행한다.
`take` : 입력한 수만큼 emit
## replay 를 제거하면
observable이 유지되지 않고 처음부터 다시 시작함
## autoConnect 를 제거하면
connect 를 호출해야만 시작함


## VolleyDemoFragment
volley 를 사용해 데이터 호출 구현
`just`와 `defer` 를 사용해서 observable 을 생성한다.
`defer` : observable subscribe 할 때, 생성한다.
null 을 리턴하지 않기 위해 사용함



## PaginationAutoFragment
subject를 사용한 paging 구현
Rxbus 로 이벤트를 받을 때 마다 subject 에 현재 page 번호를 넘긴다.
subject 에서는 page 번호부터 10개의 데이터를 추가한다.
adpater 에 데이터를 추가하고 list를 갱신한다.
## 이벤트 발생 방법
현재 list의 마지막 position 의 view 가 보이면 다음 데이터를 호출할 수 있도록
Rxbus.send 이벤트를 날린다.
## 이벤트 추가 방법
실제로 네트워크 데이터를 조회하는 것이 아니라, delay 를 2초 주고 임의로 데이터를 추가한다.
## 중간에 10개가 추가되는 이유는?
화면을 껏다 켜면 0부터 가져온다.
onStart 를 다시 호출하기 때문에 onNext(0) 을 호출해서 그런거임



## NetworkDetectorFragment
Network 연결 상태 모니터링
PublishProcessor 사용
`startWith` : 항목을 emit 시작하기 전에 입력한 값을 배출한다. (제일 처음에 호출)
`distinctUntilChanged` : 항목이 변화가 생기지 않으면 배출하지 않는다.



## UsingFragment
Kotlin
`using` : Observable과 동일한 수명을 가진 일회용 자원(객체) 생성
 using -> setup, use and dispose. Think DB connections (like Realm instances),
 socket connections, thread locks etc.



## MulticastPlaygroundFragment
Kotlin
Multicast 구현
### publish().refCount()
ConnectableObservable 변환해서 Hot observable 로 만들고 ,
refCount() 를 호출해서 Observable 변환
이 후, subscribe 에 대해서 Multicast  
### publish().autoConnect(2)
### replay(1).autoConnect(2)
### replay(1).refCount()
### replayingShare
jakewharton library
