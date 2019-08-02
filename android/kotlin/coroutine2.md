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