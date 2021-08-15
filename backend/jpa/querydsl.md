# Spring boot jpa 에 query dsl 적용하기


query dsl 사용 방법 정리

# Setting
## dependency
dependency 설정 후, `compileJava` 를 실행하면 `src/main/generated` 에 Q클래스들이 생성된다.
```groovy
// gradle 5.0 이상 & intellij 2020.x
plugins {
    id 'io.spring.dependency-management' version '1.0.10.RELEASE'
}
...

apply plugin: "io.spring.dependency-management"
dependencies {
    compile("com.querydsl:querydsl-core") // querydsl
    compile("com.querydsl:querydsl-jpa") // querydsl
    annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa" // querydsl JPAAnnotationProcessor 사용 지정
    annotationProcessor("jakarta.persistence:jakarta.persistence-api")
    annotationProcessor("jakarta.annotation:jakarta.annotation-api")

}

def generated='src/main/generated'
sourceSets {
    main.java.srcDirs += [ generated ]
}

tasks.withType(JavaCompile) {
    options.annotationProcessorGeneratedSourcesDirectory = file(generated)
}

clean.doLast {
    file(generated).deleteDir()
}
```

## Configuration
`JPAQueryFactory` 를 주입받아 Querydsl 을 사용할 수 있다.
```java
@Configuration
public class QuerydslConfiguration {

    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}
```

# Usage
다음 entity 를 예시로 Querydsl 사용방법을 설명한다.
```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class Academy {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String address;

    @Builder
    public Academy(String name, String address) {
        this.name = name;
        this.address = address;
    }
}

public interface AcademyRepository extends JpaRepository<Academy, Long> {
}
```
Querydsl Repository 를 생성한다.
```java
@Repository
public class AcademyRepositorySupport extends QuerydslRepositorySupport {
    private final JPAQueryFactory queryFactory;

    public AcademyRepositorySupport(JPAQueryFactory queryFactory) {
        super(Academy.class);
        this.queryFactory = queryFactory;
    }

    public List<Academy> findByName(String name) {
        return queryFactory
                .selectFrom(academy)
                .where(academy.name.eq(name))
                .fetch();
    }

}
```
위 코드를 작성하다보면 `academy` 를 사용할 수 없어서 에러가 발생한다. 
`compileQuerydsl` task 를 실행해서 QClass 를 생성해야 한다.
task 가 완료되면 모든 entity의 QClass 가 생성된다. `src/main/generated` 밑으로 생성된다.
`AcademyRepositorySupport` 클래스에 `QAcademy.academy` 를 import 하면 에러가 사라진다.



# fetch join
JPA Lazy Loading N+1 문제를 방지하기 위해 사용된다.
join fetch 는 JPQL 에서 제공한다.
```java
jpaQueryFactory.selectFrom(user)
    .innerJoin(user.phone, phone)
    .fetchJoin()
    .fetch();
```


# ref
[what is jpa querydsl](https://sup2is.github.io/2020/10/20/what-is-jpa-query-dsl.html)
[Spring Boot Data Jpa 프로젝트에 Querydsl 적용하기](https://jojoldu.tistory.com/372)