# CoroutineContext
The CoroutineContext is a set of attributes that configures the coroutine.
It can define the threading policy, exception handler, etc.
The main elements are the job of the coroutinn.

Coroutine context includes a coroutine dispatcher that determines what thread or threads the corresponding coroutine uses for its execution.


# CoroutineScope
A CoroutineScope can take a CoroutineContext as a parameter.

# CoroutineDispatcher

# Exception Handling
https://kotlinlang.org/docs/reference/coroutines/exception-handling.html
```kotlin
val handler = CoroutineExceptionHandler { _, exception -> 
        println("Caught $exception") 
}
val job = GlobalScope.launch(handler) {
    throw AssertionError()
}
```
# Channel

# Dispatcher?
파라미터 없이 launch 를 사용하면 부모 coroutinescope 의 context 와 dispatcher 를 상속받는다.

Dispatchers.Default 는 GlobalScope 에서 launch 를 시킨것과 동일하다.
이 coroutine 은 공유해서 사용하는 thread pool 을 사용한다.
- launch(Dispathers.Default){...}
- GlobalScope.launch{...}
따라서 이 코드는 동일하게 시작한다고 할 수 있다





# ref
[channel guide 번역](https://medium.com/@kimtaesoo188/guide-to-kotlin-coroutines-channels-part-5-7-576aee8a8f5f)
[channel guide 번역](https://tourspace.tistory.com/156) : 보기가 편함