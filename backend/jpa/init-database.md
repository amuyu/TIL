
# JPA 환경에서 DB 초기화
데이터베이스를 초기화하는 방법을 설명합니다.

* Hibernate 가 entity 를 기반으로 스키마 설정
* schema.sql 사용

## jpa 를 사용해서 database 초기화하기 위한 옵션
`spring.jpa.generate-ddl=true` 로 설정하면 `@Entity` 가 명시된 클래스를 찾아서 ddl 을 생성하고 실행
`spring.jpa.hibernate.ddl-auto` ddl 생성에 대한 옵션
- none: 자동 생성하지 않음
- create: 항상 다시 생성
- create-drop: 시작 시 생성 후 종료 시 제거
- update: 시작 시 Entity 클래스와 DB 스키마 구조를 비교해서 DB쪽에 생성되지 않은 테이블, 컬럼 추가 (제거는 하지 않음)
- validate: 시작 시 Entity 클래스와 DB 스키마 구조를 비교해서 같은지만 확인 (다르면 예외 발생)

## Spring boot 기능 사용
`spring.datasource.initialization-mode` 를 always 로 설정해야 외장 DB 초기화 가능
이 때, hibernate 가 스키마를 생성하지 않도록 비활성화하려면 `spring.jpa.ddl-auto=none` 설정한다.
```properties
spring.datasource.initialization-mode=never # Property for Spring boot 2.0
spring.datasource.initialize=false # Property for Spring boot 1.0
```

claspath 루트에 `schema.sql` 파일이 있다면 서버 시작시 스크립트를 실행한다.




## ref
[스프링 부트 - 초기 데이터 로드](https://cnpnote.tistory.com/entry/SPRING-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-%EC%B4%88%EA%B8%B0-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C%EB%93%9C)
[JPA-Hibernate 고유 기능 사용](https://velog.io/@owljoa/%EC%88%98%EC%A0%95%ED%95%84%EC%9A%94-JPA-Hibernate-%EC%B4%88%EA%B8%B0-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%83%9D%EC%84%B1-)
[초기화 레퍼런스 문서](https://docs.spring.io/spring-boot/docs/2.1.x/reference/html/howto-database-initialization.html)