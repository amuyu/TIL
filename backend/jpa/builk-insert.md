
jpa 를 이용해서 bulk insert 하는 방법 정리

1. application.yml 설정
2. ID 생성 전략 설정


# application.yml 설정
```
spring:
  jpa:
    properties:
      hibernate:
        jdbc.batch_size: 100
        order_inserts: true
        order_updates: true
  datasource:
    hikari:
      data-source-properties:
        rewriteBatchedStatements: true
```




# ref
[JPA Hibernate Bulk Save, Multi save](https://mycup.tistory.com/246)
[Spring Data Jpa Bulk Insert 하기](https://bottom-to-top.tistory.com/51)
[Performance Difference Between save and saveAll in Spring Data](https://www.baeldung.com/spring-data-save-saveall)
[JdbcTemplate 을 사용하여 batch insert 기능 구현](https://wave1994.tistory.com/160)
[Auto Increment 에서 TypeSafe Bulk Insert 진행하기](https://jojoldu.tistory.com/558)
