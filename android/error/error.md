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
