

spring-boot-starter 에서 사용하는 depedency Version 들을 변경할 수 있다.
예를 들어서 log4j2 이나 logback version 을 변경하고 싶다면
```groovy
ext['log4j2.version'] = '2.15.0'
ext['logback.version'] = '1.2.9'
```

# ref
[customizing managed versions](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#managing-dependencies.dependency-management-plugin.customizing)
[Spring-document](https://docs.spring.io/spring-boot/docs/2.6.1/reference/htmlsingle/#dependency-versions.properties)