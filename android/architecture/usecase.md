# UseCase?

```java
// synchronous
public interface UseCase {
  Response execute(Request request);
}

// asynchronous
public interface UseCase {
  void execute(Request request, Callback callback);
  interface Callback {
    onResponse(Response response);
  }
}

// asynchronous with rxjava
public interface UseCase<REQUEST, RESPONSE> {
  Observable<RESPONSE> execute(REQUEST request);
}
```

## 참고
[Android and Clean Architecture: The Use Case Interface](https://medium.com/@yoelglusc/android-and-clean-architecture-the-use-case-interface-8716512f29a1)
