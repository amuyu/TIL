# android coroutine setting
- kotlinx-coroutines-core : Main interface for using coroutines in Kotlin
- kotlinx-coroutines-android : Support for the Android Main thread in coroutines
```groovy
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:x.x.x"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:x.x.x"
```

# 코루틴?
논블록킹 비동기 코드를 마치 일반적인 동기 코드처럼 간단하게 사용할 수 있게 해주는 라이브러리

- 여러 개의 입구(entry point)를 허용하여 실행 재개가 가능
- 멈춤 시 돌아갈 위치 지정

일반적인 서브루틴은 호출될 때마다 메소드의 처음부터 실행을 시작하고 리턴될 때 항상 호출자로
돌아가는 반면, 코루틴은 이전 상태를 기억하고 있어서 호출 시 이전 호출에서 멈춘 위치에서부터
다시 실행을 재개하게 되고 멈출 때는 호출자로 돌아가는 대신 돌아갈 위치를 지정할 수 있습니다.

협력형 멀티태스킹을 구현하는 용도로 사용된다.

**coroutines are light-weight threads.**
They are launched with launch coroutine builder.

coroutines 는 thread를 block 하지 않고 suspend 할 수 있다.
coroutines suspension 은 context switch 나 OS 개입은 필요하지 않다. 사용자 라이브러리에 의해 suspension 을 제어할 수 있습니다.

coroutine 은 무작위 지시에 중단될 수 없고, 특별히 지정된 함수에 호출되는 정지 점에서만 중단되다.

# Basic
## first coroutine
다음 코드를 실행하면,
```kotlin
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
        println("World!") // print after delay
    }
    println("Hello,") // main thread continues while coroutine is delayed
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive
}
```
다음 결과를 얻을 수 있다.
```text
Hello,   # main thread
World!   # DefaultDispatcher-worker-1
```

coroutine 은 `CoroutineScope` 라 부르는 context 영역 안에서 launch(coroutine builder) 라는 것으로 실행한다.

GlobalScope 는 최상위 CoroutineScope 로 whole application 의 lifetime 를 따라간다.
왜 whole application 이라고 할까?

GlobalScope.lauch == thread, delay == Thread.sleep 과 같은 결과를 얻는다고 보면된다.

### GlobalScope.launch
launch 는 다음과 같이 작성되어 있다. 
```kotlin
// Builders.common.kt
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyStandaloneCoroutine(newContext, block) else
        StandaloneCoroutine(newContext, active = true)
    coroutine.start(start, coroutine, block)
    return coroutine
}
```
`context` 와 `start` 는 별도로 명시를 하지 않을 경우, `EmptyCoroutineContext` 와 `CoroutineStart.DEFAULT` 를 사용한다.
`CoroutineStart.DEFAULT` 는 즉시 스케줄에 등록한다.

block code 에서 `CoroutineScope.()` 는 receiver type 을 나타낸다. 
lambda 표현 방식으로 `Function literals with receiver` 에 해당한다.
수신자에게 수신자 객체를 제공한다. 수신자 객체는 this 로 호출해서 사용한다.
anonymous 형태의 extention function 과 유사한 형태이다.

Context 는 어떤 객체를 핸들링하기 위한 접근 수단의 의미를 가진다.
객체를 핸들링 하기 위해서는 해당 객체의 메모리영역, 레지스터의 값등 리소스들이 필요하다.

예를 들어 Print 를 하기 위해 PrintContext 객체를 사용하는 것을 말한다.
Context 는 프로그램이 메모리에 올라갔을 때, 실행되기 위해서 필수로 필요한 메모리영역, 레지스터의 값등의 리소스를 총칭한다.


## Briding blocking and non-blocking worlds
`runBlocking` builder 를 사용해서 blocking 을 대해서 명시할 수 있다.

```kotlin
import kotlinx.coroutines.*

fun main() { 
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main thread continues here immediately
    runBlocking {     // but this expression blocks the main thread
        delay(2000L)  // while we delay for 2 seconds to keep JVM alive
    } 
    return
}
```

main thread 는 `runBlocking` coroutine 이 끝날 때까지 block한다


### Suspending functions
suspension 은 `supsend` 예약어를 사용해 만든다.
```kotlin
suspend fun doSomething(foo: Foo): Bar { ... }
```
이와 같이 작성한 함수를 suspending functions 이라고 한다.
일반 함수와 동일한 방식으로 매개변수 및 반환 값을 취한다.
coroutines 및 다른 suspending functions 에서 호출할 수 있다.
함수 literals 도 함수에 인라인 될 수 있다.

coroutine 을 시작하기 위해서는 suspending 함수가 필요하다.
예를 들어 `async` 함수는 다음과 같다.
```kotlin
fun <T> async(block: suspend () -> T)
```
`async` 는 regular function 이고 `suspend` 함수 type 의 파라미터를 받는다.
`async()` 는 다음과 같이 lambda 로 호출할 수 있다.
```kotlin
async {
    doSomething(foo)
    ...
}
```
`await()` 은 computation이 끝날 때까지 coroutine 을 suspend 하는 suspending function 이다.
```kotlin
async {
    ...
    val result = computation.await()
    ...
}
```
`await()` 과 `doSomethind()` 과 같은 suspending functions 은
non-coroutine 함수에서 호출할 수 없다.

@RestrictsSuspension
라이브러리 사용자가 coroutine 을 중단하는 새로운 방법을 추가하는 것 방지한다.

### fun
- launch : coroutine builder, return job
- async : new coroutine, return deferred
- future : CompletableFuture


## 개념이해
### python 코루틴
yeild : 호출자(Caller)에게 컬렉션 데이타를 하나씩 리턴할 때 사용한다

# kotlin.coroutines
라이브러리로  Rx, CompletableFuture, NIO, JavaFx 및 Swing 위에서 동작한다.
- launch async, produce, actor, etc coroutine builders
- job and deferred light-weight future with cancellation support
- commonpool and other coroutine contexts
- channel and mutex communication and synchronization primitives
- delay, yield, etc top level suspending functions
- select expression support and more

# 사용예
launch 을 사용한 asynchronous computation 예
```kotlin
launch {
    // suspend while asynchronously reading
    val bytesRead = inChannel.aRead(buf)
    // we only get to this line when reading completes
    ...
    ...
    process(buf, bytesRead)
    // suspend while asynchronously writing   
    outChannel.aWrite(buf)
    // we only get to this line when writing completes  
    ...
    ...
    outFile.close()
}
```
future 을 사용한 예
```kotlin
val future = future {
    val original = asyncLoadImage("...original...") // creates a Future
    val overlay = asyncLoadImage("...overlay...")   // creates a Future
    ...
    // suspend while awaiting the loading of the images
    // then run `applyOverlay(...)` when they are both loaded
    applyOverlay(original.await(), overlay.await())
}
```
ui update 예
```
launch(UI) {
    try {
        // suspend while asynchronously making request
        val result = makeRequest()
        // display result in UI, here Swing context ensures that we always stay in event dispatch thread
        display(result)
    } catch (exception: Throwable) {
        // process exception
    }
}
```
async / await, yield 및 유사한 프로그래밍 패턴을 지원한다

## light-weight threads
coroutine은 launch를 사용해서 간단하게 thread를 구현할 수 있다.
```kotlin
fun main(args: Array<String>) {
    launch(CommonPool) { // create new coroutine in common thread pool
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
        println("World!") // print after delay
    }
    println("Hello,") // main function continues while coroutine is delayed
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive
}
```
coroutine 은 light-weight 하기 때문에 다음과 같은 구현이 가능하다.
100K 코 루틴을 시작하고, 1초후에 각 코 루틴이 .을 찍는다.
스레드로 시도하면 대부분의 경우 코드에서 일종의 메모리 부족 오류가 발생할 것이다.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val jobs = List(100_000) { // create a lot of coroutines and list their jobs
        launch(CommonPool) {
            delay(1000L)
            print(".")
        }
    }
    jobs.forEach { it.join() } // wait for all jobs to complete
}
```

## daemon threads
GlobalScope 에서 실행 된 활성 코 루틴 은 프로세스를 유지하지 않습니다. 그들은 데몬 스레드와 같습니다.
데몬 쓰레드는 다른 일반 쓰레드(데몬 쓰레드가 아닌 쓰레드)의 작업을 돕는 보조적인 역할을 수행하는 쓰레드이다.

```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    GlobalScope.launch {
        repeat(1000) { i ->
            println("I'm sleeping $i ...")
            delay(500L)
        }
    }
    delay(1300L) // just quit after delay
}
```


## runBlocking
Main coroutine

## Waiting for a job
job 을 사용해서 coroutine 이 작업이 완료될 때까지 대기할 수 있다.
```
fun main(args: Array<String>) = runBlocking<Unit> {
    val job = launch(CommonPool) { // create new coroutine and keep a reference to its Job
        delay(1000L)
        println("World!")
    }
    println("Hello,")
    job.join() // wait until child coroutine completes
    println("end!")
}
```

## Cancelation
Job은 cancel 할 수 있다.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val job = launch(CommonPool) {
        repeat(1000) { i ->
            println("I'm sleeping $i ...")
            delay(500L)
        }
    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancel() // cancels the job
    delay(1300L) // delay a bit to ensure it was cancelled indeed
    println("main: Now I can quit.")
}
```
coroutine 에서 computation loop 로 동작중 일 때는 job.cancel로 취소되지 않는다.
`isActive` 같은 cancel 상태 정보를 갖는 property를 사용해야 한다.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val startTime = System.currentTimeMillis()
    val job = launch(CommonPool) {
        var nextPrintTime = startTime
        var i = 0
        while (isActive) { // cancellable computation loop
            // print a message twice a second
            if (System.currentTimeMillis() >= nextPrintTime) {
                println("I'm sleeping ${i++} ...")
                nextPrintTime += 500L
            }
        }
    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancel() // cancels the job
    delay(1300L) // delay a bit to see if it was cancelled....
    println("main: Now I can quit.")
}
```

## Structured concurrency
`GlobalScope.launch` 의 경우, 최상위 coroutine 으로 만들어진다.
coroutine 은 light-weight 하지만 실행하는 동안 자원(메모리 등)을 사용한다.
coroutine 에 대한 참조를 유지하지 않으면 계속 실행하는 상태 일 수 있다.
모든 coroutines 에 대한 참조를 유지하고 join 하는 것은 error 가 발생할 확률이 높다.

이러한 문제를 해결하기 위해서 `structured concurrency` 를 사용할 수 있다.
특정 scope 에서 동작하는 coroutine 을 실행할 수 있다.

모든 coroutine builder 는 code block 의 scope 에 `CoroutineScope` instance 를 추가할 수 있다.
main function 을 runBlocking coroutine builder 로 coroutine 으로 만들고 이 code block scope 에서
coroutine 실행하면 outer corountine 이 runBlocking 이기 때문에 code block scope 에서 실행한 coroutine 이 
complete 될 때까지 runBlocking coroutine 이 완료되지 않는다.

> children coroutine 이 complete 될 때까지 complete 하지 않는다.

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking { // this: CoroutineScope
    launch { // launch a new coroutine in the scope of runBlocking
        delay(1000L)
        println("World!")
    }
    println("Hello,")
}
```

## Timeout
withTimeout을 사용하면 설정한 delay time 후 job을 종료한다.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    withTimeout(1300L) {
        repeat(1000) { i ->
            println("I'm sleeping $i ...")
            delay(500L)
        }
    }
}
```

## Composing suspending functions
### Concurrent using async
async 는 lauch 와 비슷하다. launch는 job을 리턴하지만 async 는 Deferred 를 리턴한다. Deferred의 .await을 사용해서 결과를 얻을 수 있다.
```kotlin
suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}

fun main(args: Array<String>) = runBlocking<Unit> {
    val time = measureTimeMillis {
        val one = async(CommonPool) { doSomethingUsefulOne() }
        val two = async(CommonPool) { doSomethingUsefulTwo() }
        println("The answer is ${one.await() + two.await()}")
    }
    println("Completed in $time ms")
}
```
두 개의 coroutines 동시에 실행되기 때문에 빠르다.



## Suspending function
delay 같은 명령을 함수내에서 사용하기 위해서는 Suspend 예약어와 함께 사용한다.
```kotlin
suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
```
suspend 예약어를 붙이지 않으면 컴파일 에러가 발생한다

## launch
create new coroutine in common thread pool
coroutine 을 생성하고 그 안에서 돌리나? 넴

You can achieve the same result replacing launch(CommonPool) { ... }
with thread { ... } and delay(...) with Thread.sleep(...). Try it.

## delay
That is because delay is a special suspending function that does not block a thread,
 but suspends coroutine and it can be only used from a coroutine.

## runBlocking
current thread 를 block함
runBlocking { ... } works as an adaptor that is used here to start the top-level main coroutine.
The regular code outside of runBlocking blocks, until the coroutine inside runBlocking is active.

## suspend
suspend coroutine 에서 사용히가 위해서 붙이는 예약어?
suspend 키워드는 어쨌던 중단/재개 지점으로 사용될 coroutine을 지정하는 의미가 되는 것 같다.
suspend가 적용된 함수는 다른 suspend 혹은 코루틴에서 호출되어야 하는데,
Suspending functions can be used inside coroutines just like regular functions,
but their additional feature is that they can, in turn, use other suspending functions,

## 참고
[코틀린 1.1 릴리즈](http://kotlin.kr/2017/03/01/kotlin-1-dot-1.html)
[coroutines-guide](https://github.com/Kotlin/kotlinx.coroutines/blob/master/coroutines-guide.md)
[guide 변역](https://medium.com/@kimtaesoo188/guide-to-kotlin-coroutines-coroutine-basics-part-1-35215c510c3d)
[번역](https://github.com/myungpyo/study-kotlin/wiki/Coroutines-(ko))
[kotlin/coroutines](https://github.com/Kotlin/kotlinx.coroutines)
[wiki/coroutine](https://en.wikipedia.org/wiki/Coroutine)
[kotlinKorea/coroutine](https://github.com/kotlin-korea/Study-Log/issues/42)
[kotlin-coroutines-guide](https://blog.simon-wirtz.de/kotlin-coroutines-guide/)
[coroutine-recipes](https://medium.com/@kimtaesoo188/kotlin-weekly-63-android-coroutine-recipes-e077cb5f3d97)
  - launch 와 async 사용

### 파이썬
[python의 corountine](http://haerakai.tistory.com/36)
[파이썬 코루틴](http://pyengine.blogspot.kr/2011/07/python-coroutine-1.html)
[파이썬 제너레이터와 코루틴](https://soooprmx.com/archives/5622)
