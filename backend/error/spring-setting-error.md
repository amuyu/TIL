
# spring boot 2.3.x starter 에서 javax.validation.* missing error
https://github.com/spring-projects/spring-boot/issues/21512

spring boot starter 2.3.x 부터 `javax.validation.*` 을 지원하지 않는다.
필요한 경우, 다음 dependency 를 추가한다.
```groovy
dependencies {
  ...
  implementation 'org.springframework.boot:spring-boot-starter-validation'
}
```
[solution](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.3-Release-Notes#validation-starter-no-longer-included-in-web-starters)