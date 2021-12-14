# 로깅 설정
jpa sql 쿼리 로깅 설정하는 방법
```yaml
spring:
  jpa:
    properties:
      hibernate:
        show_sql: true
        format_sql: true	

logging:
  level:
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE
    org.hibernate.SQL: DEBUG
    org.springframework.jdbc.core.StatementCreatorUtils: TRACE
    com.querydsl.jpa: DEBUG
```



# ref
https://jessyt.tistory.com/20