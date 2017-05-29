# Setup
## build.gradle
```groovy
compile 'com.facebook.stetho:stetho:1.5.0
compile 'com.facebook.stetho:stetho-okhttp3:1.5.0' \\ network
compile 'com.facebook.stetho:stetho-js-rhino:1.5.0' \\ javascript console
```
## initialization
```java
public class MyApplication extends Application {
  public void onCreate() {
    super.onCreate();
    Stetho.initializeWithDefaults(this);
  }
}
```
## Network inspection
```
new OkHttpClient.Builder()
    .addNetworkInterceptor(new StethoInterceptor())
    .build()
```

# Usage
setup 후, 앱을 실행하고 크롬 브라우저에서 'chrome://inspect' 로 접속하면 연결된 폰 목록과 프로젝트 목록이 보인다.
설정한 프로젝트 하단 inspect를 클릭하면 크롬 브라우저에서 디버깅을 할 수 있다

## 참고
[stetho](https://github.com/facebook/stetho)
