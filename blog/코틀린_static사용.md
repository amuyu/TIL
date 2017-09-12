# java의 static 사용하기
Companion object로 자바의 static 과 동일하게 사용할 수 있다.
## Companion Object
Companion object 를 내에 선언한 변수와 메서드는 java 에서 사용하는 static 형태로
사용할 수 있다. companion object 내에 변수와 메서드 하나를 작성해보자
```kotlin
class Key(val value: Int) {
    companion object {
        val COMPARATOR: Comparator<Key> = compareBy<Key> { it.value }
        fun bar() {}
    }
}
```
위와 같이 작성하면 아래와 같은 형태로 호출할 수 있다.
```kotlin
Key.COMPARATOR
Key.bar()
```
## java 에서 static field 호출
java에서 코틀린 위의 변수와 메서드를 호출하기 위해서는 @JvmField, @JvmStatic annotation 이 필요하다.
다음과 같이 field 키워드 앞에 annotation 을 붙여준다.
```kotlin
class Key(val value: Int) {
    companion object {
        @JvmField val COMPARATOR: Comparator<Key> = compareBy<Key> { it.value }
        @JvmStatic fun bar() {}
    }
}
```
위와 같이 작성하면 java에서 아래와 같은 형태로 호출할 수 있다.
```java
Key.COMPARATOR;
Key.bar();
```

## 참고
[코틀린에는 static이 없다?](http://kunny.github.io/lecture/kotlin/2016/07/10/kotlin_companion_object/)
[Calling Kotlin from Java](https://kotlinlang.org/docs/reference/java-to-kotlin-interop.html)
