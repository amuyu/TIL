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
