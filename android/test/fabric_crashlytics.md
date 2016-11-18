# Crashlytics 적용
모든 크래시에 대해 리포팅해주는 서비스
GA와 비교했을 때, Crash 보고가 더 빨리 올라가고 누락이 적다 
## 가입
[Crashlytics](https://try.crashlytics.com) 사이트에 가입한다.
## 빌드 설정
project.build
```gradle
buildscript {
  repositories {
    maven { url 'https://maven.fabric.io/public' }
  }

  dependencies {
    // The Fabric Gradle plugin uses an open ended version to react
    // quickly to Android tooling updates
    classpath 'io.fabric.tools:gradle:1.+'
  }
}
```
app.build
```gradle
apply plugin: 'io.fabric'

repositories {
  maven { url 'https://maven.fabric.io/public' }
}
dependencies {
compile('com.crashlytics.sdk.android:crashlytics:2.6.5@aar') {
  transitive = true;
}
```
Add API key
```xml
<meta-data
    android:name="io.fabric.ApiKey"
    android:value="7c1d7d07b82c05b71c57d2e37aae318bfbaea5f7"
/>
```
[Install Crashlytics via Gradle](https://fabric.io/kits/android/crashlytics/install)
## 코드 초기화
If you have an Application subclass, then you can place Fabric.with() in the onCreate() method. Otherwise, if you have multiple launch activities in your app, then add Fabric.with() to each launch activity. Fabric is only initialized the first time you call start, so calling it multiple times won’t cause any issues.
```java
Fabric.with(this, new Crashlytics());
```
## Configure Proguard
```
-keepattributes *Annotation*
-keepattributes SourceFile,LineNumberTable
-keep public class * extends java.lang.Exception

-keep class com.crashlytics.** { *; }
-dontwarn com.crashlytics.**
```
## 테스트
코드에서 exception을 발생시켜 확인해볼 수 있다.
```java
throw new RuntimeException("test");
```

## 참고
[fabric documentation](https://docs.fabric.io/android/fabric/overview.html)
