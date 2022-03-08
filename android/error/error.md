## Conlict with depeendency support-annotation
Gradle build will fail if the main APK and the test APK use the different version
### error log
Error:Conflict with dependency 'com.android.support:support-annotations'. Resolved versions for app (24.1.0) and test app (23.1.1) differ. See http://g.co/androidstudio/app-test-app-conflict for details.
### solution
Add the following lines in app/build.gradle
```gradle
configurations.all {
  resolutionStrategy {
    force 'com.android.support:support-annotations:23.1.1'
  }
}
```

### 참고
[Resolving conflicts between main and test APK](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/user-guide#TOC-Resolving-conflicts-between-main-and-test-APK)
[Resolved versions for app (22.0.0) and test app (21.0.3) differ](http://stackoverflow.com/a/37390580/6811452)
===========================

## Multiple dex files error
jar 중복일 경우 중복된 라이브러리를 제거한다
> What went wrong:
Execution failed for task ':homeNetworkV3_SN:transformClassesWithDexForDebug'.
com.android.build.api.transform.TransformException: com.android.ide.common.process.ProcessException: java.util.concurrent.ExecutionException: com.android.dex.DexException: Multiple dex files define Lcom/todoroo/aacenc/AACEncoder;  

### 참고
[android Fix ~~ Multiple dex files define~~Error 해결방법](http://ralf79.tistory.com/575)

===========================

## ndk error
Your project contains C++ files but it is not using a supported native build system
[stackoverflow](https://stackoverflow.com/a/40400058/6759520)
```groovy
android {
  sourceSets {
          main {
              jni.srcDirs = []
          }
      }
}
```

==========================
## AAPT - libpng error: Not a PNG file
```groovy
aaptOptions
{
    cruncherEnabled = false
}
```
[stackoverflow](https://stackoverflow.com/a/32883431/6759520)

==========================
# Manifest merger failed : uses-sdk:minSdkVersion 15 cannot be smaller than version
Manifest 다음의 내용을 추가한다.
```xml
<uses-sdk tools:overrideLibrary="packagename of library"/>
```

[stackoverflow](https://stackoverflow.com/a/26967369/6759520)
  
  
==========================
# Caused by: java.lang.Exception: No native library is found for os.name=Mac and os.arch=aarch64. path=/org/sqlite/native/Mac/aarch64

```
If you are using Apple M1 chip

One of the release notes they have mentioned by jetpack (Version 2.4.0-alpha03 )

Fixed an issue with Room’s SQLite native library to support Apple’s M1 chips.
Change Version to 2.4.0-alpha03 or above

implementation "androidx.room:room-runtime:2.4.0-alpha03"
annotationProcessor "androidx.room:room-compiler:2.4.0-alpha03"
kapt 'androidx.room:room-compiler:2.4.0-alpha03'
Reference

https://developer.android.com/jetpack/androidx/releases/room#version_240_2
```

[stackoverflow](https://stackoverflow.com/a/69142381)


61ef8c624c9c016f35c159c8
78990b52125b4c7abcbd048c45a4c082