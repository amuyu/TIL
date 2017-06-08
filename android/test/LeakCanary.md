
## Usage
```java
public class ExampleApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    if (LeakCanary.isInAnalyzerProcess(this)) {
      // This process is dedicated to LeakCanary for heap analysis.
      // You should not init your app in this process.
      return;
    }
    LeakCanary.install(this);
    // Normal app init code...
  }
}
```

## download
```groovy
dependencies {
  debugCompile 'com.squareup.leakcanary:leakcanary-android:1.5.1'
  releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.5.1'
}
```

## LeakCanary
[Android Memory Leak 해결을 위한 LeakCanary](http://dev2.prompt.co.kr/68)
[leakcanary](https://github.com/square/leakcanary)
