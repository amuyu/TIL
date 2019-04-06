# Subject
`Observable<R>를 상속받고 Observer`를 구현하는 추상 클래스입니다.
observer and observable 두 가지 역할을 수행한다.
```java
public abstract class Subject<T, R> extends Observable<R> implements Observer<T> {
  ...
}
```
- 하나 이상의 observable를 구독할 수 있음
- 항목들을 재배출하거나 새로운 항목을 배출 할 수 있음  

Observable은 누군가 Subscribe 하기 전에 이벤트가 발생하지 않는다. (Cold)
Subject는 Subscribe 와 상관없이 이벤트를 발생시킬 수 있다. (Hot)

# subject, observable 비교
subject bridge/proxy Observable 이라고 생각하면 쉽다.
subject 는 multicast, observable 은 unicast 방식



## Subject를 사용하면
observable과 observer 두 가지 인터페이스를 사용할 수 있기 때문에 좀 더 쉽게 개발할 수 있다.
아마도 Subject를 사용하는 이유는 observer가 다수일 때 처리를 위해서인듯..(multicast 같은..)
subscriptoin 공유라는 용어를 사용해서 설명하는 듯


## 언제 Subject를 사용하나?
1. What is the source of the notifications?
source가 외부냐 내부냐
내부의 source는 코드 상에서 데이터가 변화한다.
외부일 때는 subject를 사용하지 않는다.
내부일 때는 subject를 사용한다.
2. Must observers share subscription side-effects or not?
side-effects 공유하지 않으면 subject를 사용하지 않는다. (cold)
side-effects를 공유하면 subject를 사용한다. (hot)

# 종류
RelaySubject : 버퍼 사이즈에 따라 받을 수 있는 갯수가 다름
AsyncSubject : complete 되고 난뒤 source observable의 마지막 데이터를 받는다.
behaviorSubject : source observable 의 마지막 데이터 소스를 받는다.

```java
PublishSubject<Integer> source = PublishSubject.create();

// It will get 1, 2, 3, 4 and onComplete
source.subscribe(getFirstObserver());

source.onNext(1);
source.onNext(2);
source.onNext(3);

// It will get 4 and onComplete for second observer also.
source.subscribe(getSecondObserver());

source.onNext(4);
source.onComplete();
```

# 참고
[To Use Subject Or Not To Use Subject?](http://davesexton.com/blog/post/To-Use-Subject-Or-Not-To-Use-Subject.aspx)
- 어떤 경우에 Subject를 사용해야 하는가에 대한 내용 설명
[RxJava의 PublishSubject 알아보기](https://academy.realm.io/kr/posts/rxjava-publish-subject/)
[Subject 종류 알아보기](http://arnoldyoo.tistory.com/18)
[understanding-rxjava-subject](https://blog.mindorks.com/understanding-rxjava-subject-publish-replay-behavior-and-async-subject-224d663d452f)
