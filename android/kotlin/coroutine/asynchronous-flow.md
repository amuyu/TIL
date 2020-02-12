# Asynchronous Flow
Suspending function 은 single value 를 리턴한다.
비동기적으로 여러 values 들을 return 하기 위해서는 어떻게 해야 할까?

## Representing multiple values
`collections` 을 사용해서 Mutiple values 를 표현할 수 있다.
`List` 와 `forEach` 를 사용하며 다음과 같다.
```kotlin
fun foo(): List<Int> = listOf(1, 2, 3)
 
fun main() {
    foo().forEach { value -> println(value) } 
}
```
result
```text
1
2
3
```

### Sequences
cpu 를 소비하는 blocking 코드로 연산한 결과를 표현하기 위해서 `Sequence` 를 사용할 수 있다.

Sequence 는 generator 의 역할을 하는 interface
```kotlin
fun foo(): Sequence<Int> = sequence { // sequence builder
    for (i in 1..3) {
        Thread.sleep(100) // pretend we are computing it
        yield(i) // yield next value
    }
}

fun main() {
    foo().forEach { value -> println(value) } 
}
```

### Suspending funcitons
위의 코드는 main thread 를 block 한다. 이를 비동기 코드로 변환해보자
우선 foo function 에 suspend modifier 를 추가한다.
```kotlin
//sampleStart
suspend fun foo(): List<Int> {
    delay(1000) // pretend we are doing something asynchronous here
    return listOf(1, 2, 3)
}

fun main() = runBlocking<Unit> {
    foo().forEach { value -> println(value) } 
}
//sampleEnd
```

### Flows
위는 비동기긴 하지만 List<Int> 를 한번에 return 한다.
위의 `Sequence<Int>` 처럼 stream 형태로 구형하기 위해서 `Flow<Int>` 를 사용한다.
```kotlin
//sampleStart               
fun foo(): Flow<Int> = flow { // flow builder
    for (i in 1..3) {
        delay(100) // pretend we are doing something useful here
        emit(i) // emit next value
    }
}

fun main() = runBlocking<Unit> {
    // Launch a concurrent coroutine to check if the main thread is blocked
    launch {
        for (k in 1..3) {
            println("I'm not blocked $k")
            delay(100)
        }
    }
    // Collect the flow
    foo().collect { value -> println(value) } 
}
//sampleEnd
```
이 코드는 main thread 를 blocking 하지 않고 각 숫자를 print 한다
`emit` 으로 값을 내보내고 `collect` 로 값을 받았다.

### Flows are cold
flow 는 cold stream 이다. `collect` 가 호출 되기 전까지 flow builder 안에 있는 코드를 실행하지 않는다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

//sampleStart      
fun foo(): Flow<Int> = flow { 
    println("Flow started")
    for (i in 1..3) {
        delay(100)
        emit(i)
    }
}

fun main() = runBlocking<Unit> {
    println("Calling foo...")
    val flow = foo()
    println("Calling collect...")
    flow.collect { value -> println(value) } 
    println("Calling collect again...")
    flow.collect { value -> println(value) } 
}
//sampleEnd
```

### Flow cancellation
flow 는 일반적인 coroutine 의 취소 방법을 준수한다.
flow collection 은 suspending function 으로 일시 중단 되면 flow 를 취소할 수 있다.
다음은 withTimeoutOrNull 로 Flow 가 취소되는 것을 볼 수 있다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

//sampleStart           
fun foo(): Flow<Int> = flow { 
    for (i in 1..3) {
        delay(100)          
        println("Emitting $i")
        emit(i)
    }
}

fun main() = runBlocking<Unit> {
    withTimeoutOrNull(250) { // Timeout after 250ms 
        foo().collect { value -> println(value) } 
    }
    println("Done")
}
//sampleEnd
```

### Flow builders
`flow {...}` builder 는 기본 생성 방법이고 좀 더 쉬운 방법들 이 있다.
- `flowOf` builder 는 set value 들을 emit 한다.
- `.asFlow()` 는 collections 과 sequences 를 flow 로 변환한다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun main() = runBlocking<Unit> {
//sampleStart
    // Convert an integer range to a flow
    (1..3).asFlow().collect { value -> println(value) }
//sampleEnd 
}
```


### Intermediate flow operators
flow 는 collections 와 sequences 처럼 operator 로 변환할 수 있다.
intermediate operators 는 upstream 에 적용되고 downstream 을 반환한다.
operators 는 cold 이다. Flow 와 동일하게

기본 operators 는 `map`과 `filter` 이다. Sequence 와 차이점은
이러한 연산자 내 코드 block 에도 suspend function 을 호출 할 수 있다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

//sampleStart           
suspend fun performRequest(request: Int): String {
    delay(1000) // imitate long-running asynchronous work
    return "response $request"
}

fun main() = runBlocking<Unit> {
    (1..3).asFlow() // a flow of requests
        .map { request -> performRequest(request) }
        .collect { response -> println(response) }
}
//sampleEnd
```

#### Transform operator
operators 중에서 가장 일반적인 애가 `transform` 이다.
복잡한 변환부터 간단한 변환에도 사용할 수 있다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

suspend fun performRequest(request: Int): String {
    delay(1000) // imitate long-running asynchronous work
    return "response $request"
}

fun main() = runBlocking<Unit> {
//sampleStart
    (1..3).asFlow() // a flow of requests
        .transform { request ->
            emit("Making request $request") 
            emit(performRequest(request)) 
        }
        .collect { response -> println(response) }
//sampleEnd
}
```

#### Size-limiting operators
`take` 는 limit 에 도달하면 flow 의 실행을 취소한다.

coroutine 이 취소되면 exception 을 수행하므로 `try {} finally {}` 이 정상적으로 작동한다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

//sampleStart
fun numbers(): Flow<Int> = flow {
    try {                          
        emit(1)
        emit(2) 
        println("This line will not execute")
        emit(3)    
    } finally {
        println("Finally in numbers")
    }
}

fun main() = runBlocking<Unit> {
    numbers() 
        .take(2) // take only the first two
        .collect { value -> println(value) }
}            
//sampleEnd
```

### Terminal flow operators
terminal operators 는 flow collections 을 시작하는 suspending function 이다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun main() = runBlocking<Unit> {
//sampleStart         
    val sum = (1..5).asFlow()
        .map { it * it } // squares of numbers from 1 to 5                           
        .reduce { a, b -> a + b } // sum them (terminal operator)
    println(sum)
//sampleEnd     
}
```

### Flows are sequential
flow 들은 일반적으로 순서대로 실행한다.
collections terminal 연산자를 호출하는 coroutine 에서 동작하고 새로운 coroutine 을 시작하지 않는다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun main() = runBlocking<Unit> {
//sampleStart         
    (1..5).asFlow()
        .filter {
            println("Filter $it")
            it % 2 == 0              
        }              
        .map { 
            println("Map $it")
            "string $it"
        }.collect { 
            println("Collect $it")
        }    
//sampleEnd                  
}
```

### Flow context
flow collection 은 항상 호출하는 coroutine 의 context 에서 실행된다.
```kotlin
withContext(context) {
    foo.collect { value ->
        println(value) // run in the specified context 
    }
}
```

#### Wrong emission withContext
CPU 자원을 오랫동안 잡아먹는 code 는 `Dispatchers.Default` 로 UI 를 updated 하는 code 는 `Dispatchers.Main` 를 사용하는 것이 일반적이다.
그리고 상황에 따라 `withContext` 를 사용해서 적절하게 코드의 context 를 변경하지만 `flow {}` 빌더의 코드는 context 보존 특수성을 준수해야 하면
다른 context 를 emit 하면 안된다.

```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*
                      
//sampleStart
fun foo(): Flow<Int> = flow {
    // The WRONG way to change context for CPU-consuming code in flow builder
    kotlinx.coroutines.withContext(Dispatchers.Default) {
        for (i in 1..3) {
            Thread.sleep(100) // pretend we are computing it in CPU-consuming way
            emit(i) // emit next value
        }
    }
}

fun main() = runBlocking<Unit> {
    foo().collect { value -> println(value) } 
}            
//sampleEnds
```
위와 같이 호출하면 다음의 exception 이 발생한다.
```text
Exception in thread "main" java.lang.IllegalStateException: Flow invariant is violated:
		Flow was collected in [CoroutineId(1), "coroutine#1":BlockingCoroutine{Active}@5511c7f8, BlockingEventLoop@2eac3323],
		but emission happened in [CoroutineId(1), "coroutine#1":DispatchedCoroutine{Active}@2dae0000, DefaultDispatcher].
		Please refer to 'flow' documentation or use 'flowOn' instead
	at ...
```

#### flowOn operator
flow 의 emission context 를 변경하려면 `flowOn` function 을 사용하면 된다.
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
           
//sampleStart
fun foo(): Flow<Int> = flow {
    for (i in 1..3) {
        Thread.sleep(100) // pretend we are computing it in CPU-consuming way
        log("Emitting $i")
        emit(i) // emit next value
    }
}.flowOn(Dispatchers.Default) // RIGHT way to change context for CPU-consuming code in flow builder

fun main() = runBlocking<Unit> {
    foo().collect { value ->
        log("Collected $value") 
    } 
}            
//sampleEnd
```

### Buffering
collector 에서 처리하는 시간이 오래 걸릴 경우, (emit 시간 대비)
