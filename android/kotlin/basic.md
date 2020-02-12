
# Function literals with receiver
`A.(B) -> C` 는 receiver 를 갖고 있는 function type 을 나타낸다.
수신자에게 수신자 객체를 제공한다. 수신자 객체는 this 로 호출해서 사용한다.
extention function 과 유사한 형태이다. (anonymous function)
```kotlin
val sum: Int.(Int) -> Int = { other -> plus(other) }
val result = 3.sum(2)
```


