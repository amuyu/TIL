# RxJava
## ReactiveX 란?
비동기처리와 이벤트 기반의 프로그램 개발을 위해 유용한 기능을 제공
ReactiveX는 Observer패턴과 Iterator패턴, 그리고 함수형 프로그래밍의 아이디어를 조합한 형태
### Observer 패턴
Obsever pattern은 객체의 상태 변화를 관찰하는 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해
객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴. 주로 분산 이벤트 핸들링 시스템 구현에 사용된다.
발행/구독 모델로 알려져 있기도 하다.
- Subject(Observable) : 이벤트를 발생시킨다.
- Observer(Subscriber) : 발생된 이벤트를 받아 처리한다.  
RxJava의 Observable는 누군가 구독(subscribe)하고 있지 않는다고 하면 이벤트를 발생시키지 않는다.  

### 용어 정리
- event : 구독자들에게 전달되는 데이터 의미, (클릭이벤트, 상태이벤트, API응답) - 발생시점
- subscribe : subscriber가 이벤트를 전달받기 위해 하는 행위, 이벤트를 받은 후 처리하는 내용을 정의함
- observe : 구독보다는 좀 더 넓은 범위의 행위
  예 : Observable 컴포넌트들을 서로 연결하는 과정에서 Observable은 다른 Observable을 관찰한다.

### 기본 Observable 구조
observe와 subscribe 기본 구조를 살펴보자
```java
// observe
Observable<String> simpleObservable =
    Observable.create(new Observable.OnSubscribe<String>() {
        @Override
        public void call(Subscriber<? super String> subscriber) {
            subscriber.onNext("Hello RxAndroid !!");
            subscriber.onCompleted();
        }
    });
// subscribe
simpleObservable
    .subscribe(new Subscriber<String>() {
        @Override
        public void onCompleted() {
            Log.d(TAG, "complete!");
        }

        @Override
        public void onError(Throwable e) {
            Log.e(TAG, "error: " + e.getMessage());
        }

        @Override
        public void onNext(String text) {
            ((TextView) findViewById(R.id.textView)).setText(text);
        }
    });
```
subscribe 는 편의를 위해 간단한 인터페이스를 제공한다
다음은 onNext만 다루고 있다
```java
simpleObservable
    .subscribe(new Action1<String>() {
        @Override
        public void call(String text) {
            ((TextView) findViewById(R.id.textView)).setText(text);
        }
    });
```
onNext, onError를 다루는 구성도 있다
RestAPI 호출을 RxAndroid와 사용하는 경우에는 onNext와 onError 구성을 많이 사용한다.

### Observable과 Subscriber 사용해보기
Observable은 이벤트를 발생시키는 주체로 하나 혹은 여러개의 이벤트를 발생시키고
최종적으로 이벤트의 종료를 알리거나 에러 발생을 알린다.

RxJava에서 이벤트의 발생, 종료, 에러는 아래와 같이 표현된다
- onNext : 이벤트의 발생
- onCompleted : 이벤트 종료
- onError : 에러 발생

#### Obeservable 이벤트 발생
##### Observable.just
누군가가 subscribe 하게되면 이벤트를 한 번 발생시킴 (기본)
##### Observable.from
배열 또는 Iterable의 요소를 순서대로 이벤트로 발생시킴
##### Observable.defer
subscribe 하면 특정 function을 실행하고 subscriber에 이벤트를 전달함

#### 비동기 처리를 위한 subscribeOn과 observeOn
Observable.defer()와 subscribeOn, observeOn을 사용하면 쉽게 비동기 처리를 할수 있음
##### subscribeOn
observable에 subscribe 이루어지는 thread
##### observeOn
Subscriber(observer)에서 이벤트를 처리할 때 사용되는 thread
Observable이 이벤트를 전파할 때, 사용되는 thread

### 스케줄러
스케줄러는 observable, subscribe, operator 를 어떤 스레드에서 수행할지 결정하는 것 subscribeOn과 observeOn으로 지정
#### 스케줄러 목록
- Schedulers.computation : 간단한 연산이나 콜백 처리를 위해 사용 I/O 처리를 여기에서 해서는 안됨
- Schedulers.from(executor) : 특정 executor를 스케줄러로 사용
- Schedulers.immediate : 현재 스레드에서 즉시 수행
- Schedulers.io : 동기 I/O를 별도로 처리시켜 비동기 효율을 얻기 위한 스케줄러, 자체 스레드 풀에 의존함
- Schedulers.newThread : 항상 새로운 스레드를 만드는 스케줄러
- Schedulers.trampoline : 큐에 있는 일이 끝나면 이어서 현재 스레드에서 수행하는 스케줄러
- AndroidSchedulers.mainThread : 안드로이드의 UI 스레드에서 동작
- HandlerScheduler.from : 특정 핸들러 thread 에 의졶나여 동작
안드로이드에서는 AndroidSchedulers.mainThread 와 RxJava가 제공하는 Schedulers.io 를 조합해서 많이 사용한다

### Observable 만들기
#### create
create()에 넘겨지는 객체는 OnSubscribe 라는 인터페이스를 구현한다
어떤 형태로 이벤트가 발생되고 전파되는지를 정의하는 인터페이스
```java
Observable.create(subscriber-> {
	subscriber.onNext("hi");
}).subscribe(
	text -> System.out.println("onNext : " + text),
    throwable -> System.out.println("onError : " + throwable.getMessage()),
    () -> System.out.println("onCompleted")
);
```
#### interval
interval()은 주기적으로 이벤트를 전파한다. 타임워치나 특정 주기마다 같은 일을 반복해야 할 필요성이 있을 때, 주로 사용한다.
```java
Observable.interval(1, TimeUnit.SECONDS)
            .observeOn(Schedulers.computation())
            .subscribe(count -> {
                System.out.println("onNext : " + new Date());
            });
```
#### timer
timer()는 특정시간만큼 지나서 이벤트를 발생시킨다
```java
Observable.timer(10, TimeUnit.SECONDS)
            .observeOn(Schedulers.computation())
            .subscribe(
                count -> System.out.println("onNext : " + new Date()),
                e -> System.out.println("onError : " + new Date()),
                () -> System.out.println("onCompleted : " + new Date())
            );
```
#### range
range() 시작점과 반복횟수를 지정하면 반복해서 이벤트를 발생시킨다
```java
Observable.range(10, 10)
            .observeOn(Schedulers.computation())
            .subscribe(
                count -> System.out.println("onNext : " + count),
                e -> System.out.println("onError"),
                () -> System.out.println("onCompleted")
            );
```

### Observable 변형하기 (Operator)
발생한 이벤트를 다른 형태로 변형하기,  Operator 만들기
#### map
함수를 사용하여 전달받은 이벤트를 다른값으로 변경
```java
Observable.from(new String[] { "개미", "매", "마루" })
            .map(text -> "** " + text + " **")
            .subscribe(
                text -> System.out.println("onNext : " + text),
                e -> System.out.println("onError"),
                () -> System.out.println("onCompleted"));

// 결과
onNext : ** 개미 **
onNext : ** 매 **
onNext : ** 마루 **
onCompleted
```
#### flatMap
전달받은 이벤트로부터 다른 Observable을 생성하고, 발생한 이벤트 전파
```java
Observable.from(new String[] { "개미", "매", "마루" })
            .flatMap(
                text -> Observable.from(new String[] { text + " 사랑합니다.", text + " 고맙습니다." })
            )
            .subscribe(
                text -> System.out.println("onNext : " + text),
                e -> System.out.println("onError"),
                () -> System.out.println("onCompleted"));

// 결과
onNext : 개미 사랑합니다.
onNext : 개미 고맙습니다.
onNext : 매 사랑합니다.
onNext : 매 고맙습니다.
onNext : 마루 사랑합니다.
onNext : 마루 고맙습니다.
onCompleted
```
##### flatMap과 concatMap 비교
concatMap은 concat 연산자를 사용하고 이벤트 스트림을 통해 전달된 항목 하나에 대한 처리가 완료된 후에 다음 항목을 처리한다. 순서가 뒤바뀌지 않는다.
flatMap은 merge 연산자를 사용하고 이벤트 스트림에 항목 순서와 출력되는 순서가 달라질 수 있다.

### Observable 합성하기
두 개 이상의 Observable을 합성
#### zip
예를 들어 네트워크 작업으로 사용자의 프로필과 프로필 이미지를 동시에 요청하고, 그 결과를 합성해서 화면에 표현해준다거나 하는 형태의 작업
```java
Observable.zip(
            Observable.just("개미"),
            Observable.just("gaemi.jpg"),
            (profile, image) -> "프로필 : " + profile + ", 이미지 : " + image
        ).subscribe(
            print -> System.out.println("onNext : " + print),
            e -> System.out.println("onError"),
            () -> System.out.println("onCompleted")
        );

// 결과
onNext : 프로필 : 개미, 이미지 : gaemi.jpg
onCompleted
```

## Subject 사용하기
Subject는 이벤트를 전달받아 구독자들에게 이벤트를 전파하는 중간다리
Android 에서는 EventBus와 같은 형태로 사용 가능하다. RxJava를 사용하면 다른 EventBus 라이브러리가 불필요해진다.
Subject들은 보통 onCompleted와 같이 종료하는 과정이 없이, 액티비티 라이프 사이클과 동일하게 살아서 이벤트를 전파하는 역할로 사용된다.
### PublishSubject
PublishSubject를 구독한 시점으로부터 이후에 발생하는 이벤트를 전달받는다
```java
PublishSubject<문자> subject = PublishSubject.create();  

subject.onNext("첫번째 호출");
subject.onNext("두번째 호출");

subject.subscribe(text -> {
    System.out.println("onNext : " + text);
});

subject.onNext("세번째 호출");
subject.onNext("네번째 호출");

// 결과
onNext : 세번째 호출
onNext : 네번째 호출
```

### BehaviorSubject
구독 전에 한 건이라도 이벤트가 발생했다면 구독시점에 해당 이벤트도 같이 전달받습니다.
```java
BehaviorSubject<문자> subject = BehaviorSubject.create();

subject.onNext("첫번째 호출");
subject.onNext("두번째 호출");

subject.subscribe(text -> {
    System.out.println("onNext : " + text);
});

subject.onNext("세번째 호출");
subject.onNext("네번째 호출");

// 결과
onNext : 두번째 호출
onNext : 세번째 호출
onNext : 네번째 호출
```

## 왜 RxJava를 도입했는가?
여러 문제 해결과 리팩토링을 목적으로
- 안드로이드 비동기 처리나 그 외 에러 핸들링 처리가 귀찮다
- 리스트 작업이나 람다식을 쓰는게 소스 코드 가독성이 높다
- 리팩토링을 통해 데이터 소스가 변경되었을 때, 화면의 갱신을 이벤트로 처리하도록 한다.

## RxJava를 사용한 해결책
### 리스트 처리가 아직도 for문으로 처리된다
map()이나 filter()를 사용해서 for loop를 사용하지 않는 리스트 처리가 가능해짐 new 나 add 같은 절차적인 코드가 사라지고 리스트에 대한 가공만 코드에 나타나서 읽기가 쉽다
```java
// Java7
List<스트링> otherUserNames = new ArrayList<>(users.size);
for (User user : users) {
    if (user.id != selfUser.id) {
        otherUserNames.add(user.getName());
    }
}

// Java8
List<스트링> otherUserNames = users.stream()
    .filter(user -> user.id != selfUser.id)
    .map(User::getName) // user -> user.getName()의 단순 표현, method references라고 부릅니다.
    .collect(Collectors.toList());

// RxJava
List<스트링> otherUserNames = Observable.from(users)
    .filter(user -> user.id != selfUser.id)
    .map(User::getName)
    .toList().toBlocking().single();
    // 전부 리스트로 묶어서, 동기 처리할 수 있도록 설정하고, 하나로 결과를 합칩니다.
```
### 비동기 처리가 귀찮은 문제
백그라운드 처리를 실행하기 위한 패턴들은 귀찮은 부분들이 있다.
메모리 누수, cancel, 에러 핸들링, 셋팅이 어렵다
```java
observable
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(result -> render(result));
```
Retrofit과 RxJava 사용한 예, 메모리 누수 없이 심플하게 코드를 작성할 수 있다.
```java
public class UserListFragment extends Fragment {
    private Subscription mSubscription;
    private Client mClient;
    ...

    public void onStart() {
        super.onStart();
        mSubscription = mClient.users()
            .observeOn(AndroidSchedulers.mainThread())
            // 결과의 통지는 UI Thread에서 실행됩니다.
            .subscribe(                                
            // subscribe의 타이밍에 처리가 시작되어 Callback에 결과가 보내집니다.
                this::render,                          
                // 결과를 render 메소드에 전달합니다.
                error -> showErrorAlert(error),        
                // 에러가 발생할 때의 예외 처리를 지정해줍니다.（optional）
                () -> showCompletedAlert());           
                // 정상적으로 처리가 끝났을 때의 처리를 지정해줍니다. (optional)
    }
    // ※ mainThread()는 RxAndroid의 구현을 참조할 것.

    public void onStop() {
        // View가 사라지기 전에 처리가 끝나지 않았어도 unsubscript를 통해 취소하고, callback으로 넘어가지 않도록 합니다.
        // (Observable이 바르게 구현되어 있다면) 참조가 사라지면 Activity나 Fragment가 메모리 누수를 일으키지 않습니다.
        // 어느 라이프사이클에서 subscribe나 unsubscribe할지 베스트 케이스가 정해지지 않았습니다. (역주: 있으면 공유를 부탁드립니다...)
        mSubscription.unsubscribe();
        super.onStop();
    }

    private void render(List<User> userList) {
        ...
    }

    ...
}
```

## 변경 알림 흐름을 쫓기 어려워진 문제
표시할 때마다 Fragment에서 Model을 호출하는 장소에서 변경 알림 이벤트에 의한 View의 갱신을 가하면 호출의 흐름이 복잡하게 되어 버린다. EventBus를 통해서 구현하면 그 이벤트가 어디서 날아오는지 명확히 알 수 없다.
이것을 Observer 패턴을 이용하면 변경 내용이 통지될 때마다 render하는 것만으로도 괜찮아지며, 또 subscribe 할 때에 Model을 참조하기 때문에 가독성이 높아진다.

## 구현 예시
- 리스트 작업 : 스트림 처리, Operator, toBlocking()
- 비동기 처리 : 리스트 처리 + subscribe() + unsubscribe(), Scheduler, (필요에 따라서) Observable의 자작 방법
- 변경 통지 : 비동기 처리 + Subject + 가능하면 Backpressure


## 사용하기 위한 설정
### gradle.build
```gradle
compile 'io.reactivex:rxandroid:1.2.1'
```
### proguard
```gradle
# rxjava
-keep class rx.schedulers.Schedulers {
    public static <methods>;
}
-keep class rx.schedulers.ImmediateScheduler {
    public <methods>;
}
-keep class rx.schedulers.TestScheduler {
    public <methods>;
}
-keep class rx.schedulers.Schedulers {
    public static ** test();
}
-keepclassmembers class rx.internal.util.unsafe.** {
    long producerIndex;
    long consumerIndex;
}
#-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
#    long producerIndex;
#    long consumerIndex;
#}
#-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
#    long producerNode;
#    long consumerNode;
#}
-dontwarn rx.**
```

# RxBinding
안드로이드에서 필수적인 obserable은 RxBinding에 구현되어 있다
## build.gradle
```gradle
compile 'com.jakewharton.rxbinding:rxbinding:0.3.0'
```
## RxView
가장 유용한 도구는 RxView.clicks이다 RxView.clicks는 view 타입을 인자로 받는 정적 메서드로 setOnClickListener를 통해 OnClickListener에 전달될 이벤트를 옵저버블 형태로 래핑한다
```Java
RxView
    .clicks(findViewById(R.id.button))
    .map(event -> new Random().nextInt())
    .subscribe(value -> {
        TextView textView = (TextView) findViewB yId(R.id.textView);
        textView.setText("number: " + value.toString());
    }, throwable -> {
        Log.e(TAG, "Error: " + throwable.getMessage());
        throwable.printStackTrace();
    });
```
### merge
여러 경로로 온 이벤트를 동시에 처리해야 하는 경우, 개별 이벤트를 옵저버블로 받은 후 병합을 시도할 수 있다 다음은 두 개의 버튼에서 이벤트를 병합하여 처리하는 예이다
```Java
Observable<String> lefts = RxView.clicks(findViewById(R.id.leftButton))
        .map(event -> "left");

Observable<String> rights = RxView.clicks(findViewById(R.id.rightButton))
        .map(event -> "right");

Observable<String> together = Observable.merge(lefts, rights);

together.subscribe(text -> ((TextView) findViewById(R.id.textView)).setText(text));

together.map(text -> text.toUpperCase())
        .subscribe(text -> Toast.makeText(this, text, Toast.LENGTH_SHORT).show());
```
An onError notification from any of the source Observables will immediately be passed through to observers and will terminate the merged Observable. 

### scan
병합된 데이터를 누적으로 처리할 수 있다
다음은 두 가지 scan을 예로 들었는데, 첫번째 scan은 together 옵저버블을 통해 전달되는 값을 number로 받지만 전혀 사용하고 있지 않고 1씩 증가한다
두번째 sacn은 number로 값을 받는 것은 동일하지만 플러스 버튼을 누르면 1이 더해지고 마이너스 버튼을 누르면 -1이 감소한다 두 가지 scan 모두 값을 누적해서 처리한다   
```Java
Observable<Integer> minuses = RxView.clicks(findViewById(R.id.minusButton))
    .map(event -> -1);
Observable<Integer> pluses = RxView.clicks(findViewById(R.id.plusButton))
    .map(event -> 1);
Observable<Integer> together = Observable.merge(minuses, pluses);
together.scan(0, (sum, number) -> sum + 1)
    .subscribe(count ->
        ((TextView) findViewById(R.id.count)).setText(count.toString()));
together.scan(0, (sum, number) -> sum + number)
    .subscribe(number ->
        ((TextView) findViewById(R.id.number)).setText(number.toString()));
```

# TIP
## MissingBackpressureException
Observable에서 항목을 보내는 속도보다 처리하는 속도가 느릴 때 발생한다.
RxAndroid를 사용하는 경우, 수신된 데이터를 UI 표시하기 위해 observeOn(AndroidSchedulers.mainThread())  를 많이 사용하는데,
이 과정에서 UI 쓰레드 내에서 다른 작업이 수행되고 있어 Observable에서 보낸 항목을 바로 처리하지 못할 경우 MissingBackpressureException 발생
### 해결책
- onBackpressureBuffer() : Observable에서 보낸 항목을 큐에 계속 쌓아두어, 항목을 처리하는 쪽에서 해당 항목을 나중에 처리할 수 있도록 한다.
- onBackpressureDrop() : Observable에서 항목을 보냈을 때 바로 처리되지 못한 데이터는 무시

## 참고
[RxJava의 PublishSubject 알아보기](https://realm.io/kr/news/rxjava-publish-subject/)
[RxJava를 3일만에 입문해서, 안드로이드 애플리케이션의 리스트 작업이나 비동기 처리와 알림을 해결한 이야기](http://realignist.me/code/2016/05/29/rxjava-on-android.html)
[RxJava사용해보기](http://gaemi.github.io/android/2015/05/20/RxJava%20with%20Android%20-%201%20-%20RxJava%20%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0.html)
[Java 8: No More loops를 안드로이드 환경에서 RxJava로 해보자.](http://realignist.me/code/2016/05/29/rxjava-on-android.html)
-> 근데 일어라...
[리액티브 프로그래밍 도입기:사운드 클라우드 아키텍처](https://realm.io/kr/news/gotocph-mattias-kappler-reactive-architecture-android/)
[Retrofit2.0과 RxJava(Observable)사용](http://blog.naver.com/PostView.nhn?blogId=artisan_ryu&logNo=220629095154)
[안드로이드 동시성 프로그래밍](http://www.slideshare.net/deview/1b4-39616041)
[RxAndroid로 리액티브 앱 만들기](https://realm.io/kr/news/rxandroid/)
[RxJava/RxAndroid](http://kunny.github.io/community/2016/02/08/gdg_korea_android_weekly_02_1/)
[ReactiveX Wiki](http://reactivex.io/documentation/operators/merge.html)
