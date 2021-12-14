# dependency
```groovy
implementation 'org.springframework.boot:spring-boot-starter-websocket'
```

# websocket handler
websocket 에서 받은 메시지를 처리할 handler
```java
@Slf4j
@Component
public class WebSocketHandler extends TextWebSocketHandler {

    @Override
    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        log.debug("message {}", message.getPayload());
        log.debug("session id : {}", session.getId());
    }
}
```

# Configure
websocket server 구동을 위한 configure
```java
@RequiredArgsConstructor
@EnableWebSocket
@Configuration
public class WebSocketConfig implements WebSocketConfigurer {

    final WebSocketHandler webSocketHandler;

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(webSocketHandler, "/ws/test").setAllowedOrigins("*");
    }
}
```





# nettysocket io
https://github.com/jamesjieye/netty-socketio.spring
https://krcoder.com/p/307703


# mod-socket-io
https://hamait.tistory.com/201
