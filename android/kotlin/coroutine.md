# 코루틴?
논블록킹 비동기 코드를 마치 일반적인 동기 코드처럼 간단하게 사용할 수 있게 해주는 라이브러리

- 여러 개의 입구(entry point)를 허용하여 실행 재개가 가능
- 멈춤 시 돌아갈 위치 지정

일반적인 서브루틴은 호출될 때마다 메소드의 처음부터 실행을 시작하고 리턴될 때 항상 호출자로
돌아가는 반면, 코루틴은 이전 상태를 기억하고 있어서 호출 시 이전 호출에서 멈춘 위치에서부터
다시 실행을 재개하게 되고 멈출 때는 호출자로 돌아가는 대신 돌아갈 위치를 지정할 수 있습니다.

협력형 멀티태스킹을 구현하는 용도로 사용된다.

coroutines are light-weight threads.
They are launched with launch coroutine builder.

## 개념이해
### python 코루틴
yeild

# kotlin.coroutines
라이브러리로  Rx, CompletableFuture, NIO, JavaFx 및 Swing 위에서 동작한다.
- launch async, produce, actor, etc coroutine builders
- job and deferred light-weight future with cancellation support
- commonpool and other coroutine contexts
- channel and mutex communication and synchronization primitives
- delay, yield, etc top level suspending functions
- select expression support and more

# 사용방법
```
// runs the code in the background thread pool
fun asyncOverlay() = async(CommonPool) {
    // start two async operations
    val original = asyncLoadImage("original")
    val overlay = asyncLoadImage("overlay")
    // and then apply overlay to both results
    applyOverlay(original.await(), overlay.await())
}

// launches new coroutine in UI context
launch(UI) {
    // wait for async overlay to complete
    val image = asyncOverlay().await()
    // and then show it in UI
    showImage(image)
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
thread를 사용해서 아래와 같이 구현하면 에러가 날 것이다.
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


## runBlocking
Main coroutine

## Waiting for a job
coroutine 이 작업이 완료될 때까지 대기할 수 있다.
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
[kotlin/coroutines](https://github.com/Kotlin/kotlinx.coroutines)
[wiki/coroutine](https://en.wikipedia.org/wiki/Coroutine)
[kotlinKorea/coroutine](https://github.com/kotlin-korea/Study-Log/issues/42)
[kotlin-coroutines-guide](https://blog.simon-wirtz.de/kotlin-coroutines-guide/)

### 파이썬
[python의 corountine](http://haerakai.tistory.com/36)
[파이썬 코루틴](http://pyengine.blogspot.kr/2011/07/python-coroutine-1.html)
[파이썬 제너레이터와 코루틴](https://soooprmx.com/archives/5622)