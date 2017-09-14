# Anko
Anko는 안드로이드 개발을 더 빠르고 쉽게 해주는 코틀린 라이브러리 입니다.
Anko 를 사용해서 xml을 사용하지 않고 동적으로 layout 을 만들 수 있는 라이브러리로 알려져 있는데
그 외에도 다양한 기능을 제공하고 있습니다.
이 중에서 `Anko Commons` 사용 방법을 간단히 소개하고자 합니다.

샘플 코드 : [GitHub](https://github.com/amuyu/SampleAnko/) 참고

## Kotlin
anko 를 사용하기 위해서 Kotlin 플러그인을 설정이 되어 있어야 합니다.
[Android kotlin 프로젝트 시작하기](http://muyu.tistory.com/entry/Kotlin-프로젝트-시작하기-Kotlin-Android-Extensions-사용)

## gradle 설정
anko commons 라이브러리를 사용하기 위해서 anko 와 anko-commons dependency를 추가합니다.
```groovy
// app/build.gradle
...
dependencies {
  compile "org.jetbrains.anko:anko:$ankoVersion"
  compile "org.jetbrains.anko:anko-commons:$ankoVersion"
}
...
```

## Activity 화면 이동
`startActivity`를 사용해 Activity를 호출합니다.
### 전달할 데이터가 없는 경우
```kotlin
startActivity<Activity>()
// ex.
startActivity<SomeOtherActivity>()
```
### 전달할 데이터가 있는 경우
```kotlin
startActivity<Activity>(key to value)
// ex.
startActivity<SomeOtherActivity>("id" to 5)
```

## Toast
`toast`를 사용해 Toast message를 호출합니다.
```kotlin
toast("Toast message!!!")
```

## Alert Dialog
`alert`를 Alert Dialog를 띄웁니다.
```kotlin
alert("Hi,","title") {
      yesButton { toast("확인") }
      noButton { toast("취소") }
    }.show()
```

## Progress Dialog
`progressDialog`를 progress dialog를 호출합니다.
```kotlin
progressDialog(message = "Please wait a bit…", title = "Fetching data").show()
```

## 마치며
간단하게 `Anko-commons` 를 사용해봤는데 훨씬 코드가 간결해지고 깔끔해지는 것을 볼 수 있었습니다.
좀 더 자세한 내용은 [Anko](https://github.com/Kotlin/anko) 에서 wiki 문서를 참고하시면 될 것 같습니다.



# 참고
[anko-common](https://github.com/Kotlin/anko/wiki/Anko-Commons-–-Dialogs)
