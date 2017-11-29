yanolza
# Null Safe
# Inference
# if/when/try
java : Statement : no return
kotlin : expression : return
# smart casts
```kotlin
when (x) {
    is Int -> print(x + 1)
    is String -> print(x.length + 1)
    is IntArray -> print(x.sum())
}
```
## condition
- val local variables - always;
- val properties - if the property is private or internal or the check is performed in the same module where the property is declared. Smart casts aren't applicable to open properties or properties that have custom getters;
- var local variables - if the variable is not modified between the check and the usage and is not captured in a lambda that modifies it;
- var properties - never (because the variable can be modified at any time by other code).

# Extension Function
# Higher-Order Functions and Lambdas
```kotlin
//
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
    val result = arrayListOf<R>()
    for (item in this)
        result.add(transform(item))
    return result
}
//
list.map() { x -> x * 2 }
```

# operator
apply, also, run, let
## Scope
╔══════════╦═════════════════╦═══════════════╦═══════════════╗
║ Function ║ Receiver (this) ║ Argument (it) ║    Result     ║
╠══════════╬═════════════════╬═══════════════╬═══════════════╣
║ let      ║ this@MyClass    ║ String("...") ║ Int(42)       ║
║ run      ║ String("...")   ║ N\A           ║ Int(42)       ║
║ run*     ║ this@MyClass    ║ N\A           ║ Int(42)       ║
║ with*    ║ String("...")   ║ N\A           ║ Int(42)       ║
║ apply    ║ String("...")   ║ N\A           ║ String("...") ║
║ also     ║ this@MyClass    ║ String("...") ║ String("...") ║
╚══════════╩═════════════════╩═══════════════╩═══════════════╝


# Kotlin Android Extension
## Parcelable


# Choice
## 절망편
learning curve
functional programming
hybrid language 가지는 문제들..
hangul 문서가 없음
kotlin compiler 에 의해 생성되는 코드(생각하는 것보다 더 많은 코드가 생성)
kapt, 알 수 없는 에러
자바와의 이별은 불가능
## 희망편
java 문제 해결
kotlin android Extension
kotlin 사용 증가
simple is best
support jetbrain
