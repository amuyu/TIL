## Function
function -> predicate

## Action
action -> consumer

## Subscription
subscription -> disposal

## from
from -> fromIterable, fromArray 등

## onCompleted
onCompleted -> onComplete

## asObservable
asObservable -> hide

## RxJava2Interop 사용
Rxjava 셋팅
```groovy
..
packagingOptions {
        exclude('META-INF/rxjava.properties')
    }
..

compile "io.reactivex.rxjava2:rxandroid:${rxAndroidVersion}"
compile "io.reactivex.rxjava2:rxjava:${rxVersion}"
compile "com.github.akarnokd:rxjava2-interop:0.10.2"
```

## Completable 사용

```
Completable.fromAction { callback.invoke(0) }
                    .subscribeOnMainThread()
                    .subscribe()
```

## SubscribeWith
Subscribe 에서 더는 Subscription 을 리턴하지 않는다.
SubscribeWith 를 사용해서 Disposable 을 사용해야 한다.

## 참고
[Rxjava2.x 무엇이 달라졌을까](http://realignist.me/code/2017/01/25/rxjava2-changelog.html)
[RxJava2 를 이용한 Collection 데이터 처리](http://doohyun.tistory.com/44)
[RxJava2Interop](https://github.com/akarnokd/RxJava2Interop)
[2.x: Duplicated file rxjava.properties #4445](https://github.com/ReactiveX/RxJava/issues/4445)
[RxJava2를 도입하며](https://medium.com/rainist-engineering/migrate-from-rxjava1-to-rxjava2-3aea3ff9051c)
