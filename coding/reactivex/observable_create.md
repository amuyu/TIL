
# Observable.create
Returns an Observable that will execute the specified function when a Subscriber subscribes to it.
OnSubscribe 를 입력받아 Observable을 생성한다.


## Observable
The Observable class that implements the Reactive Pattern.
This class provides methods for subscribing to the Observable as well as delegate methods to the various Observers.
이벤트 발생 시점에 수행할 메커니즘(데이터 조회/변환)을 정의(onSubscribe)
하고 이벤트를 발생시키면 준비된 연산을 실행하고 결과를 observer에게 리턴한다.
Observable은 누군가 subscribe 하는 시점에 이벤트를 발생시킨다.
(그래서 observable을 Cold라고 부른다. subscribe 하기 전에는 이벤트가 발생하지 않으므로)


## OnSubscribe
Invoked when Observable.subscribe is called.
이벤트 발생 시점에 수행할 메커니즘(데이터 조회/변환)을 정의
```java
public interface OnSubscribe<T> extends Action1<Subscriber<? super T>> {
  // cover for generics insanity
}
```
### OnSubscribeFromIterable
Converts an Iterable sequence into an Observable.
### JustOnSubscribe
The OnSubscribe callback for the Observable constructor.




## Subscriber
Provides a mechanism for receiving push-based notifications from Observables,
and permits manual unsubscribing from these Observables.
Subscriber는 observer를 구현한다.
## observer?
Provides a mechanism for receiving push-based notifications (subscriber와 동일?)
onCompleted/onError  Notify 한다, onNext emission 한다
## Action1
```java
public interface Action1<T> extends Action {
    void call(T t);
}
```
## Producer
onNext, onCompleted 호출


# just
Returns an Observable that emits a single item and then completes.
파라미터로 입력한 item 을 가지고 내부에서 JustOnSubscribe 를 생성한다.
## ScalarSynchronousObservable
An Observable that emits a single constant scalar value to Subscribers.
## JustOnSubscribe



# Hot and cold observable
## Hot observable
A “hot” Observable may begin emitting items as soon as it is created, and so any observer who later subscribes to that Observable may start observing the sequence somewhere in the middle.
subscribe 와 상관없이 item을 emit할 수 있음
이를 구현하기 위해서 subject 와 ConnectableObservable을 사용한다.
## Cold observable
A “cold” Observable, on the other hand, waits until an observer subscribes to it before it begins to emit items, and so such an observer is guaranteed to see the whole sequence from the beginning
subscribe 시점에 item이 emit 됨


# ConnectableObservable
subscribe 하는 것과 상관없이 connect 를 해주어야 item을 emit 한다. (Hot Observable 이라고 한다.)
## publish()
ConnectableObservable 을 반환한다.
## refCount(), publish()
ConnectableObservable 을 Observable로 바꿔준다.




# 궁금한거
observable의 publisher 는 누가?

# 참고
[observable](http://reactivex.io/documentation/observable.html)
[reactor pattern](https://en.wikipedia.org/wiki/Reactor_pattern)
[RxAndroid Hot & Cold Observable](https://moka-a.github.io/android/rxAndroid_study/)
[Hot and cold rx-java observable](http://www.java-allandsundry.com/2015/03/hot-and-cold-rx-java-observable.html)
