# UseCase Basie
Entities 간에 데이터 흐름을 조정합니다.
UseCase는 기본적으로 Request와 Response가 있다.
추상클래스, 인터페이스로 기본 형태를 정의해놓고 상속받아 사용한다.
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

public abstract class UseCase<Q extends UseCase.RequestValues, P extends UseCase.ResponseValue> {
    protected abstract Observable<P> executeUseCase(Q requestValues);

    public interface RequestValues {
    }

    public interface ResponseValue {
    }
}
```

# To-do cleanarchitecture UseCase
UseCase > UseCaseHandler > UseCaseScheduler
## UseCase<Request, Response>
비즈니스 룰?로직에 접근하기 위한 인터페이스

## UseCaseThreadPoolScheduler
handler, threadpoolexecutor를 사용한다.
threadpoolexecutor : 비동기 실행을 위해 사용
handler : 실행 후에 UI 에 결과를 callback 하기 위해 사용

## UseCaseHandler
UseCaseScheduler를 호출해서 UseCase를 실행한다.
UiCallbackWrapper.onSuccess > handler.notifyResponse > scheduler.notifyResponse > callback.onSuccess
## UiCallbackWrapper
UseCase 에서 UseCaseHandler에 접근하기 위해 사용

Handler 에 job을 넣는다  Handler에서 job을 실행한다.  
UseCase에 끝나는 시점은 UseCase에서 알 기 쉬우므로 작업 후에 Handler 에 끝났다고 알려준다.
Handler에 알려주기 위해 UiCallbackwrapper 을 사용한다.


## 참고
[Android and Clean Architecture: The Use Case Interface](https://medium.com/@yoelglusc/android-and-clean-architecture-the-use-case-interface-8716512f29a1)
