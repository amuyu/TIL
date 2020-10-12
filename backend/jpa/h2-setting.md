# Spring 에서 h2 database 사용하기

```yml
spring:
  datasource:
    driver-class-name: org.h2.Driver
    url: jdbc:h2:mem:testdb
    username: sa
    password: sa
    initialization-mode: embedded
    data: classpath*:db/data.sql
  h2:
    console:
      enabled: true
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    generate-ddl: true
    hibernate:
      ddl-auto: create-drop
```