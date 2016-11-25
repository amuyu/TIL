# Dagger2

## Gradle 설정
### Android.Gradle
```gradle
// Add Dagger dependencies
dependencies {
  compile 'com.google.dagger:dagger:2.x'
  annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}

// Dagger2 2.0.1
// project.gradle
classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'

// app.gradle
apply plugin: 'android-apt'

compile 'com.google.dagger:dagger:2.0.1'
apt 'com.google.dagger:dagger-compiler:2.0.1'
provided 'org.glassfish:javax.annotation:10.0-b28'
```

## 개발
### Module 생성
#### provide 추가
#### provide 호출 클래스에  @Inject 추가
### Component 생성
### @Inject 추가

## 참고
[Android 개발에서 Dagger2이용해보기.](http://drcarter.tistory.com/169)
[JakeWharton 발표자료](https://speakerdeck.com/jakewharton/dependency-injection-with-dagger-2-devoxx-2014)
[Dagger2 학습에 필요한 참고 자료](http://kunny.github.io/life/2016/06/06/dagger2_resources/)
[Android and dagger2 발표자료](http://www.slideshare.net/ssuser70b5b8/android-and-dagger2)
[google/dagger](https://github.com/google/dagger)
[Dagger Document](http://google.github.io/dagger/)
