# spring tomcat 시작 시, jackson NoClassDefFoundError 발생
에러메시지 
> nested exception is java.lang.NoClassDefFoundError: com/fasterxml/jackson/datatype/jsr310/ser/ZoneIdSerializer

jackson 라이브러리 버전 관련인데, 적절한? version 의 dependency 를 설정하거나
기존 설정한 dependency 를 제거해서 해결할 수 있다.

spring boot starter 2.3.3 에서 아래 dependency 설정도 함께 사용했는데 
```
implementation 'com.fasterxml.jackson.core:jackson-databind:2.9.8'
```
제거한 후, 정상 동작한다.
