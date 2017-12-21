anko kotlin 으로 작성된 DSL

# anko common
```groovy
dependencies {
    compile "org.jetbrains.anko:anko-commons:$anko_version"
}
```

## Toast
```kotlin
toast("Hi there!")
toast(R.string.message)
longToast("Wow, such duration")
```

## Alert
```kotlin
alert("Hi, I'm Roy", "Have you tried turning it off and on again?") {
    yesButton { toast("Oh…") }
    noButton {}
}.show()
```
Appcompat Alert
```kotlin
alert(Appcompat, "Some text message").show()
```
you need dependency
```groovy
implementation "org.jetbrains.anko:anko-appcompat-v7-commons:$ankoVersion"``
```
cusetView
```kotlin
alert {
    customView {
        editText()
    }
}.show()
```
you need dependency
```groovy
implementation "org.jetbrains.anko:anko-commons:$rootProject.ankoVersion"
```

## Intent

# 참고
[anko](https://github.com/Kotlin/anko)
[kotlin과 anko로 개발하기](https://academy.realm.io/kr/posts/getting-started-with-kotlin-and-anko/)
[anko-common](https://github.com/Kotlin/anko/wiki/Anko-Commons-–-Dialogs)
[안드로이드 anko 파헤치기](https://www.slideshare.net/moka_a/anko-in-aak)
