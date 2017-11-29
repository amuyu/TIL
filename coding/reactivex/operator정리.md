# retrywhen
에러가 발생했을 때, 처리하는 operator. retry 보다 세밀하게 제어하기 위한 operator
## 예제
다음과 같은 형태로 Observable 의 에러 발생을 catch 해서 observable 을 반복 처리할 수 있다.
```java
public class RetryWithConnection implements Function<Flowable<Throwable>, Publisher<Long>> {
    private final String TAG = RetryWithConnection.class.getSimpleName();

    private final int maxRetries = 3;
    private long currentRetryCount = 0;

    @Override
    public Publisher<Long> apply(Flowable<Throwable> throwableFlowable) throws Exception {
        return throwableFlowable
                .zipWith(Flowable.range(1, maxRetries), (throwable, number) ->throwable)
                .flatMap(new Function<Throwable, Publisher<Long>>() {
                    @Override
                    public Publisher<Long> apply(Throwable throwable) throws Exception {
                        currentRetryCount++;
                        return Flowable.timer(1 * currentRetryCount, TimeUnit.SECONDS);
                    }
                });
    }
}
```
