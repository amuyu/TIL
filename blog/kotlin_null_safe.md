# [Android] 코틀린에서 nullable 사용 - Null Safety
코틀린에서는 자바보다 간단하게 null 에 대한 확인을 할 수 있고, 안전하게 호출할 수 있다.
어떻게 하는지 방법을 살펴보자

### Safe Call - `?.`, `?:`
null 체크하는 방법을 자바와 비교해보자
어떤 String에 대한 length를 리턴하는 코드를 구현할 때,
자바에서는 다음과 같이 작성할 수 있다.
length 를 호출할 때 NullException이 발생할 수 있으므로 null인지 아닌지를 확인하는 코드가 필요하다.
```java
String text;
if (text != null) return text.length()
else return null
```
이를 코틀린에서는 safe call operator `?.`를 사용해서 다음과 같이 작성할 수 있다.
```kotlin
var text: String?
return text?.length
```
여기서 만약 null 이 아니라 0 이나 -1 을 리턴한다고 하면
elvis operator `?:` 를 사용해서 다음과 같이 수정할 수 있다.
```kotlin
var text: String?
return text?.length ?: 0
```

### let과 `?.` 사용하기
let 은 호출하는 객체를 이어지는 블록 인자로 넘겨 블록에서 이를 사용할 수 있도록 하는 함수인데
`?.` operator 와 함께 사용하면 null 이 아닌 경우에만 메소드가 실행되도록 할 수 있다.
예를 들어 String 이 null  이 아닐 경우에만 length 를 출력한다고 할 때,
자바에서는 다음과 같이 작성할 수 있다.
```java
String text;
if (text != null) {
  System.out.println(text.legnth());
}
```
코틀린에서 let 을 사용하면 다음과 같이 작성할 수 있다.
```kotlin
var text: String?
text?.let {
  println(it)
}
```

이상으로, 코틀린에서 안전하게 null 을 처리하기 위한 방법을 살짝 살펴보았는데
확실히 자바보다 null 처리가 더 편할 것 같다.
