
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


# mysql driver
mysql connector 에서 eriver 의 이름이 변경됨 `com.mysql.cj.jdbc.Driver` 를 사용하면 된다.
> The name of the class that implements java.sql.Driver in MySQL Connector/J has changed from com.mysql.jdbc.Driver to com.mysql.cj.jdbc.Driver. The old class name has been deprecated.  
https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-api-changes.html
