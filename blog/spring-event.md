
Spring 에서 비동기 ApplicationEvent 구현하는 방법을 정리하려고 합니다.

간략하게 다음과 같은 순서로 구현을 합니다.
- ApplicationEvent 를 상속받는 event 클래스
- ApplicationEventPublisher object 를 주입받아 event publish
- ApplicationListener interface 를 사용해서 listener 구현
- 비동기 이벤트 설정


1. Application Event 클래스 구현 
```java
public class CustomSpringEvent extends ApplicationEvent {
    private String message;

    public CustomSpringEvent(Object source, String message) {
        super(source);
        this.message = message;
    }
    public String getMessage() {
        return message;
    }
}
// generic
public class GenericSpringEvent<T> {
    private T what;
    protected boolean success;

    public GenericSpringEvent(T what, boolean success) {
        this.what = what;
        this.success = success;
    }
    // ... standard getters
}
```

2. Publisher
```java
@Component
public class CustomSpringEventPublisher {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishCustomEvent(final String message) {
        System.out.println("Publishing custom event. ");
        CustomSpringEvent customSpringEvent = new CustomSpringEvent(this, message);
        applicationEventPublisher.publishEvent(customSpringEvent);
    }
}
```

3. Listener
```java
@Component
public class CustomSpringEventListener implements ApplicationListener<CustomSpringEvent> {
    @Override
    public void onApplicationEvent(CustomSpringEvent event) {
        System.out.println("Received spring custom event - " + event.getMessage());
    }
}

// annotation 사용
```

4. 비동기 설정
```
@Configuration
public class AsynchronousSpringEventsConfig {
    @Bean(name = "applicationEventMulticaster")
    public ApplicationEventMulticaster simpleApplicationEventMulticaster() {
        SimpleApplicationEventMulticaster eventMulticaster =
          new SimpleApplicationEventMulticaster();
        
        eventMulticaster.setTaskExecutor(new SimpleAsyncTaskExecutor());
        return eventMulticaster;
    }
}

// Async annotation 사용
```


# 참고
[Spring Events](https://www.baeldung.com/spring-events)
[Spring Boot에서 이벤트 사용하기](https://shinsunyoung.tistory.com/88)