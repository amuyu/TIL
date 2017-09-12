# 코틀린에서 databinding 사용하기
java 에서 kotlin 으로 코드 변환 중, build 할 때는 에러가 없는데,
앱 실행할 때, 다음과 같은 에러가 발생한다.
```
Error:(13, 37) Unresolved reference: databinding
Error:(20, 35) Unresolved reference: ActivityMainBinding
Error:(28, 51) Cannot infer a type for this parameter. Please specify it explicitly.
```
kotlin 에서 databinding 을 사용하기 위해서는 플러그인과 dependency 추가가 필요하다.
app/build.gradle 에 다음과 같이 추가한다.
```groovy
apply plugin: 'kotlin-kapt'
...
dependencies {
    // compiler version 은 gradle version과 같거나 높아야 한다.
    kapt "com.android.databinding:compiler:2.3.2"
}
```
만약에 databinding 사용 중이 아니었다면 databinding 사용하는 설정도 추가한다.
```groovy
android {
  ...
  dataBinding {
      enabled = true
  }
}
```
그러면 정상적으로 실행된다.
