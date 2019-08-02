# network state
network 상태는 기본적으로 로딩, 실패, 성공으로 나누어볼 수 있는데
kotlin 에서는 이를 sealed class 를 사용하여 상태 관리로 사용할 수 있다.

# 뷰상태
화면은 상태의 집합체라고 할 수 있다.

=> 상태란? 화면에서 표시되는 정보들을 상태로 보고 있는 듯 하다.
상태는 한 화면에서의 정보의 변화 상태라고 해야 하지 않을까?

# SealedClass 란?
Kotlin 에서 제공하는 문법, 대수적 타입(Algebraic data type) 중에서 합 타입을 표현할 수 있다.

=> 대수적?
대수학은 수 대신에 문자를 사용하여 방정식의 풀이 방법이나 대수적 구조를 연구하는 학문
대수적 타입은 다른 자료형의 값을 가지는 자료형

sealed class 와 enum class는 모두 타입을 제한하고 추상화를 하는데 유용하다.
다른점은 enum class는 모두 같은 타입의 변수돠 같은 타입의 함수를 가져야 한다. 
하지만 sealed class 는 서로 다른 타입의 변수, 함수를 갖을 수 있다.

# 정리
sealed class 는 타입을 제한하는 형태로 사용하면서 내부 아이템들이 서로 다른 타입을 가질 수 있어
뷰의 상태 변화 관리에 용이하다.

```kotlin
sealed class NetworkState<out T> {
    class Init : NetworkState<Nothing>()
    class Loading : NetworkState<Nothing>()
    class Success<out T>(val item: T) : NetworkState<T>()
    class Error(val throwable: Throwable?) : NetworkState<Nothing>()
}

sealed class RefreshState {
        /**
         * The request is currently loading.
         *
         * An object is a singleton that cannot have more than one instance.
         */
        object Loading : RefreshState()

        /**
         * The request has completed successfully.
         *
         * An object is a singleton that cannot have more than one instance.
         */
        object Success : RefreshState()

        /**
         * The request has completed with an error
         *
         * @param error error message ready to be displayed to user
         */
        class Error(val error: Throwable) : RefreshState()
}

/**
 * Network result class that represents both success and errors
 */
sealed class FakeNetworkResult<T>

/**
 * Passed to listener when the network request was successful
 *
 * @param data the result
 */
class FakeNetworkSuccess<T>(val data: T) : FakeNetworkResult<T>()

/**
 * Passed to listener when the network failed
 *
 * @param error the exception that caused this error
 */
class FakeNetworkError<T>(val error: Throwable) : FakeNetworkResult<T>()

/**
 * Listener "type" for observing a [FakeNetworkCall]
 */
typealias FakeNetworkListener<T> = (FakeNetworkResult<T>) -> Unit

/**
 * Throwable to use in fake network errors.
 */
class FakeNetworkException(message: String) : Throwable(message)
```




# ref
[selad class 를 사용한 ui 상태 관리](https://medium.com/@lazysoul/kotlin-sealed-class를-사용한-ui-상태-관리-3-3-d47754a7a4b6)