
# DataObserver
```java
class DataObserver {
  Map<Object, List<Listener>> register;
  Subject notifier;

  void <T> register(Object invoker, Class klass, Listener listener);
  void unregister(Object invoker)
  void update(Result result)
}
```

## 참고
[reactive-android-application-architecture](http://www.slideshare.net/ssuser70b5b8/reactive-android-application-architecture)
