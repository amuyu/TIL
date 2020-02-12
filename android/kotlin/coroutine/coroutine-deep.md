
## Scope builder
builder 에서 제공하는 CoroutineScope 외에도 coroutineScope 빌더를 사용해서 고유한 범위를 선언할 수 있습니다.
`coroutineScope` builder 를 사용하면 coroutine scope 를 만들수 있고 모든 children 이 종료될 때까지 종료되지 않습니다.

runBlocking 과 coroutineScope 는 children 이 종료될 때까지 대기한다는 점이 비슷하다.
두 가지의 가장 큰 차이점은 runBlocking 은 현재 thread 를 block 하지만 
coroutinScope 는 일시 중단되어 다른 용도로 사용되는 기본 스레드를 해제한다는 것이다.(실행한다)


## Extract function refactoring
`launch { ... }` 내부에 code block 은 function 으로 나눌 수 있다.
일반 함수와 똑같지만 `suspend` modifier 를 사용해서 새로운 기능을 얻게 된다.

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    launch { doWorld() }
    println("Hello,")
}

// this is your first suspending function
suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
```

`suspend` 는 언제든지 지연되었다가 재개 될 수 있는 함수를 말한다. (non-blocking)

suspend function 에서 coroutineScope 를 사용하는 것은 이 메소드의 scope 위에서 동작하도록  제어할 수 없으므로 
구조적으로 안전하지 않다. private 하게 쓰거나 Class Field 로 사용하는 것을 권장한다.


## Composing suspending functions

### Sequential by default
coroutine 에서 code 는 순차적이기 때문에는 susepnd function 들을 호출하면 순서대로 호출합니다.

### Concurrent using async
suspend funcgtion 들 사이에 의존성이 없어 동시에 실행하려면 `async` 를 사용한다.
`async` 는 `launch` 와 비슷한데 차이점은 launch 가 return 하는 Job 은 결과값을 땡겨올 수 없지만
async 가 return 하는 deferred 는 결과값을 땡겨올 수 있다.

## Coroutine Context and Dispatchers
coroutine 은 항상 `CoroutineContext` 에서 실행된다.
coroutine context 는 다양한 elements 들이 있는데 주요한 elements 는 job과 dispater 가 있다.

### Dispatchers and threads
Coroutine dispatcher 는 coroutine 실행을 특정 thread 로 하거나 thread pool 에 던지거나 
제한되지 않은 형태로 사용할 수 있다.

launch 와 async 는 CoroutineContext 를 parameter 로 전달할 수 있다.

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> {
//sampleStart
    launch { // context of the parent, main runBlocking coroutine
        println("main runBlocking      : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Unconfined) { // not confined -- will work with main thread
        println("Unconfined            : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Default) { // will get dispatched to DefaultDispatcher 
        println("Default               : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(newSingleThreadContext("MyOwnThread")) { // will get its own new thread
        println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
    }
//sampleEnd    
}
```
결과
```text
Unconfined            : I'm working in thread main
Default               : I'm working in thread DefaultDispatcher-worker-1
newSingleThreadContext: I'm working in thread MyOwnThread
main runBlocking      : I'm working in thread main
```

Dispatchers.Unconfiend 는 특별한 dispatcher 이다. main thread 에서 동작하는 것처럼 보이지만 다른 메커니즘을 갖고 있다.
Dispatchers.Default 는 공유 background pool 을 사용한다.
newSingleThreadContext 는 전용 thread 를 생성한다. (자원을 많이 사용한다.)

### Unconfined vs confined dispatcher
Dispatchers.Unconfined 는 첫번째 suspension point 까지는 호출자 thread 에서 시작하고 suspension 후에는 호출한 suspending function 에 결정됩니다.
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> {
//sampleStart
    launch(Dispatchers.Unconfined) { // not confined -- will work with main thread
        println("Unconfined      : I'm working in thread ${Thread.currentThread().name}")
        delay(500)
        println("Unconfined      : After delay in thread ${Thread.currentThread().name}")
    }
    launch { // context of the parent, main runBlocking coroutine
        println("main runBlocking: I'm working in thread ${Thread.currentThread().name}")
        delay(1000)
        println("main runBlocking: After delay in thread ${Thread.currentThread().name}")
    }
//sampleEnd    
}
```
result
```text
Unconfined      : I'm working in thread main
main runBlocking: I'm working in thread main
Unconfined      : After delay in thread kotlinx.coroutines.DefaultExecutor
main runBlocking: After delay in thread main
```

Unconfined dispatchers 는 cpu time 을 소비하지 않거나 특정 thread 의 공유 data 를 변경하지 않는 coroutine 에 적합하다

default dispatchers 는 외부 CoroutineScope 을 상속받고
fifo 로 스케줄링되어 confining 하게 실행된다.

### Jumping between threads
`withContext` function 을 사용해서 coroutine 의 context 를 변경할 수 있다.
아래 코드에서는 `runBlocking` 호출 시, 특정 context 를 지정해서 실행하고 coroutine 내부에서 withContext function 으로 context 를 변경해서 실행하도록 했다.
```kotlin
import kotlinx.coroutines.*

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")

fun main() {
//sampleStart
    newSingleThreadContext("Ctx1").use { ctx1 ->
        newSingleThreadContext("Ctx2").use { ctx2 ->
            runBlocking(ctx1) {
                log("Started in ctx1")
                withContext(ctx2) {
                    log("Working in ctx2")
                }
                log("Back to ctx1")
            }
        }
    }
//sampleEnd    
}
```
result
```
[Ctx1 @coroutine#1] Started in ctx1
[Ctx2 @coroutine#1] Working in ctx2
[Ctx1 @coroutine#1] Back to ctx1
```

### Job in the context
Job 은 coroutineContext 의 부분으로, `coroutineContext[Job]` 으로 찾을 수 있다.
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> {
//sampleStart
    println("My job is ${coroutineContext[Job]}")
//sampleEnd    
}
```
result
```text
My job is "coroutine#1":BlockingCoroutine{Active}@6d311334
```

### Children of a coroutine
`CoroutineScope` 에서 다른 coroutine 을 실행하면 `CoroutineScope.coroutineContext` 를 상속하고
새로운 coroutine 의 Job 은 부모 coroutine job 의 child 가 된다.

GlobalScope 를 사용하면 새로운 coroutine 은 부모가 없는 상태가 된다.

### Parantal responsibilities
부모 coroutine 은 항상 children 이 완료될 때까지 기다린다.
children 을 추적할 필요가 없고, 끝에서 Job.join 을 호출할 필요도 없다.

### Naming coroutines for debugging
`CoroutineName` 을 사용해서 thread 이름에 이름을 지정해서 사용할 수 있다.
디버깅이 좀더 편하다.

### Combining context elements
coroutine context 를 여러 요소들로 정의하려고 하면 `+` operator 를 사용하면 된다.
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> {
//sampleStart
    launch(Dispatchers.Default + CoroutineName("test")) {
        println("I'm working in thread ${Thread.currentThread().name}")
    }
//sampleEnd    
}
```

### Coroutine scope
어떤 lifecycle 을 갖는 application 이 있다고 할 때, (예를 들어 android)
activity 가 destory 되면 coroutine 들은 cancel 되어야 한다.
이렇게 하기 위해서 acitivity 의 lifecycler 과 연관된 coroutinescope 를 만들어서 관리해야 한다.

```kotlin
class Activity {
    private val mainScope = MainScope()
    
    fun destroy() {
        mainScope.cancel()
    }
    // to be continued ...
```
interface 형태
```kotlin
class Activity : CoroutineScope by CoroutineScope(Dispatchers.Default) {

    fun destroy() {
        cancel() // Extension on CoroutineScope
    }
    // to be continued ...

    // class Activity continues
    fun doSomething() {
        // launch ten coroutines for a demo, each working for a different time
        repeat(10) { i ->
            launch {
                delay((i + 1) * 200L) // variable delay 200ms, 400ms, ... etc
                println("Coroutine $i is done")
            }
        }
    }
} // class Activity ends
```

### Thread-local data
`ThreadLocal` 을 사용하면 thread(context) 별로 데이터를 유지하고 context 전환할 때마다 복원할 수 있다.
```kotlin
val threadLocal =  ThreadLocal < 문자열? > () // 스레드 로컬 변수 선언

fun  main () = runBlocking < Unit > {
 // sampleStart 
    threadLocal.set ( " main " )
     println ( " 사전 메인, 현재 스레드 : $ { Thread .currentThread ()} , 스레드 로컬 값 : ' $ {threadLocal.get ()} ' " )
     val job = launch ( 디스패처 . 기본값  + threadLocal.asContextElement (value =  " launch " )) {
         println ( "시작 시작, 현재 스레드 : $ { Thread .currentThread ()} , 스레드 로컬 값 : ' $ {threadLocal.get ()} ' " )
         yield ()
         println ( " 수율 후, 현재 스레드 : $ { Thread .currentThread () } , 스레드 로컬 값 : ' $ {threadLocal.get ()} ' " )
    }
    job.join ()
    println ( " Post-main, 현재 스레드 : $ { Thread .currentThread ()} , 스레드 로컬 값 : ' $ {threadLocal.get ()} ' " )
 // sampleEnd     
}
```