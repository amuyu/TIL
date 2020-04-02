


## 3. 스프링 데이터 리파지토리들과 작업하기
스프링 데이터 레파지토리 추상회의 목표는 다양한 영속화 저장소를 위한 데이터 액세스 레이어를 구현하는데 있어 필요한 상당한 양의 보일러 플레이트 코드들을 줄이는데 있습니다.

### 3.1. 핵심 개념
`Repository` 인터페이스는 도메인 클래스와 도메인의 id 타입을 type arguments 로 받습니다.

`CrudRepository` 는 관리하는 entity class 에서 복잡한 CURD 기능을 제공해줍니다.
```java
public interface CrudRepository<T, ID extends Serializable>
    extends Repository<T, ID> {

    <S extends T> S save(S entity); 

    T findOne(ID primaryKey);       

    Iterable<T> findAll();          

    Long count();                   

    void delete(T entity);          

    boolean exists(ID primaryKey);  

    // … more functionality omitted.
}
```

`CrudRepository` 를 확장한 것 중에 `PagingAndSortingRepository` 는 쉽게 엔티티들에 대해 페이징을 할 수 있는 메소드를 제공합니다.
```java
public interface PagingAndSortingRepository<T, ID extends Serializable>
  extends CrudRepository<T, ID> {

  Iterable<T> findAll(Sort sort);

  Page<T> findAll(Pageable pageable);
}
```
`User` 의 페이지 사이즈를 20으로 잡고 두번째 페이지에 접근하는 것은 간단하게 이렇게 해볼 수 있습니다.
```java
PagingAndSortingRepository<User, Long> repository = // … get access to a bean
Page<User> users = repository.findAll(new PageRequest(1, 20));
```


### 3.2. Query 메소드들
표준 CRUD 기능들을 가진 레파지토리들은 데이터 소스에 대한 쿼리들을 가지고 있습니다. 쿼리들은 다음과 같은 순서로 만들 수 있습니다.

1. Repository 나 하위 인터페이스를 상속하는 인터페이스를 상속하여 도메인클래스와 ID 타입을 적어줍시다.
```java
Interface PersonRepository extends Repository<User, Long> { … }
```

2. 인터페이스에 대해서 쿼리 메소드를 선언합니다.
```java
Interface PersonRepository extends Repository<User, Long> {
	List<Person> findByLastname(String lastname);
}
```

3. 스프링을 설정하여 인터페이스를 위한 프록시 인스턴스를 생성하게 해줍니다.
자바설정
```java
Import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@EnableJpsRepositories
Class Config {}
```
Xml 설정
```xml
<?xml version=“1.0” encoding=“UTF-8”?>
<beans xmlns=“http://www.springframework.org/schema/beans”
   xmlns:xsi=“http://www.w3.org/2001/XMLSchema-instance"
   xmlns:jpa=“http://www.springframework.org/schema/data/jpa”
   xsi:schemaLocation=“http://www.springframework.org/schema/beans
     http://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/data/jpa
     http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

   <jpa:repositories base-package="com.acme.repositories"/>

</beans>
```

사용하기 위한 준비는 끝났고 다음과 같이 사용할 수 있습니다.
```java
public class SomeClient {

  @Autowired
  private PersonRepository repository;

  public void doSomething() {
    List<Person> persons = repository.findByLastname(“Matthews”);
  }
}
```


### 3.3. 리파지토리 인터페이스 정의하기
도메이니 특정 레파지토리 인터페이스를 정의해야 합니다. 이 이터페이스는 반드시 Repository 를 상속해야 하며, 도메인 클래스와 아이디 타입을 적워줘야 합니다.

#### 3.3.1. 좀 더 세밀한 리파지토리 정의 조정
리파지토리 인터페이스는 `Repository`, `CurdRepository`, `PagingAndSortingRepostiroy` 같은 것을 상속할 것입니다. 만약 스프링 데이터 인터페이스를 상속하기를 원하지 않는다면 @RepositoryDefinition 을 붙일 수 있습니다. `CrudRepository` 를 상속하는 것은 `CrudRepository` 에 속한 엔티티를 다루는 메소드들의 집합을 모두 사용합니다. 만약 `CrudRepository` 의 메소드를 선택적으로 사용하고 싶다면, `CrudRepository` 에서 필요한 부분을 복사해서 Repository 에 붙이면 됩니다.

선택적으로 CRUD 메소드 노출시키기
```java
@NoRepositoryBean
interface MyBaseRepository<T, ID extends Serializable> extends Repository<T, ID> {

  T findOne(ID id);

  T save(T entity);
}

interface UserRepository extends MyBaseRepository<User, Long> {
  User findByEmailAddress(EmailAddress emailAddress);
}
```

MyBaseRepository 에 정의한 `findOne(…)` 과 `save(…)` 는 base repository 구현체를 참조할 것입니다. (예를 들어, JPA 를 사용한다면, `SimpleJpsRepository` 같은) 왜냐하면 `CrudRepository` 의 method signature 와 일치하기 때문입니다.

### 3.4. 쿼리 메소드 정의하기
리파지토리 프록시는 메소드 이름으로 저장소에 맞는 쿼리를 만들어내는 두가지 방법이 있습니다. 하나는 메소드이름으로 직접적으로 쿼리를 만들어내는 것이고, 아니면 수동적으로 정의된 쿼리를 사용하는 것이빈다.

#### 3.4.1. 쿼리 탐색 전략
XML 설정의 네임스페이스에서 `query-lookup-strategy` 속성을 토앟거나 자바 설정에서 `queryLookupStrategy` 속성을 통해 전략을 설정할 수 있습니다.

* CREATE : 저장소에 맞는 쿼리를 이름으로부터 만들어내는 것을 시도합니다.
* USE_DECLARED_QUERY :  선언된 쿼리를 찾는 것, 찾을 수 없을 때는 예외를 날린다.
* CREATE_IF_NOT_FOUND(default) : CREATE, 와 USE_DECLARED_QUERY 를 조합합니다. 	

#### 3.4.2. 쿼리 생성
쿼리는 `find…By`, `read…By`, `query…By`, `count…By`, `get…By` 같은 접두어들을 메소드에서 떼어내고 나머지 부분을 파싱해서 만듭니다. 

다음은 `Distinct` 같은 더 싶은 표현을 포함하고 있습니다.

메소드네임으로 쿼리 생성하기
```java
public interface PersonRepository extends Repository<User, Long> {

  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);

  // Enables the distinct flag for the query
  List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
  List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

  // Enabling ignoring case for an individual property
  List<Person> findByLastnameIgnoreCase(String lastname);
  // Enabling ignoring case for all suitable properties
  List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);

  // Enabling static ORDER BY for a query
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

메소드에 파싱된 결과는 쿼리를 생성하는 데이터베이스에 따라 다르지만 일반적인 것들이 있습니다.
* `And` 나 `Or` 같은 표현과 함께 속성을 조합할 수 있습니다. `Between`, `LessThan`, `GreaterThan`, `Like` 같은 연산자의 지원을 받을 수 있습니다.
* `IgnoreCase` 같은 속성 설정을 지원합니다.
* `OrderBy` 를 추가하여 쿼리메소드의 정렬 방향을 정할 수 있습니다. `Asc` 나 `Desc` 를 사용해서 

#### 3.4.3. 속성 표현
엔티티의 직접적인 속성을 참조할 수 있습니다. 그리고 중첩속성에 대한 제약을 정의할 수 있습니다. `Person` 이 `ZipCode` 와 함께 있는 `Address` 를 가지고 있다고 생각해봅시다.

```java
List<Person> findByAddressZipCode(ZipCode zipCode);
```

이렇게 되며, x.address.zipCode 라는 속성에 접근하게 됩니다. 알고리즘(쿼리로 변경하는)은 속성에 대해서  `AdressZip` , `Code` 를 찾을 수 도 있습니다.  그런데 `Person` 이 addressZip 이라는 속성도 가지고 있다면 원래 의도했던 것과는 다른게 동작하는 쿼리가 생성됩니다. 
이러한 모호한 점을 해결하기 위해 `_` 를 메소드 이름 내에 사용하여 수동적으로 나누는 지점을 정의할 수 있습니다.
```java
List<Person> findByAddress_ZipCode(ZipCode zipCode);
```

#### 3.4.4. 특별 파라미터 핸들링
쿼리 메소드에서  `Pageable`, `Sort` 같은 특정 타입을 인식하여 쿼리에서 페이징과 정렬을 동적으로 만들어줍니다.

```java
Page<User> findByLastname(String lastname, Pageable pageable);

Slice<User> findByLastname(String lastname, Pageable pageable);

List<User> findByLastname(String lastname, Sort sort);

List<User> findByLastname(String lastname, Pageable pageable);
```

#### 3.4.5. 쿼리 결과 Limit 하기
쿼리 메소드의 결과는 `first` 나 `top` 을 통해서 제한될 수 있습니다.
```java
User findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

Slice<User> findTop3ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

Limiting 표현은 `Distinct` 키워드를 지원합니다. 

#### 3.4.6. 쿼리 결과 Streaming 하기
쿼리 메소드의 결과값은 Java8의 `Stream<T>` 를 이용하여 점차적으로 처리될 수 있습니다.

```java
@Query(“select u from User u”)
Stream<User> findAllByCustomQueryAndStream();

Stream<User> readAllByFirstnameNotNull();

@Query(“select u from User u”)
Stream<User> streamAllPaged(Pageable pageable);
```

Stream<T> 와 try-with-resources 블록
```java
try (Stream<User> stream = repository.findAllByCustomQueryAndStream()) {
  stream.forEach(…);
}
```


### 3.5. 리파지토리 인스턴스 생성하기
리파지토리 인터페이스 정의를 위한 인스턴스들을 생성해보고 빈도 정의해봅니다.

#### 3.5.1. XML configuration

#### 3.5.2. JavaConfig
리파지토리 인프라스트럭쳐는 자바설정에서 특정 저장소 `@Enable${store}Repositories` 어노테이션을 사용하여 동작합니다.

```java
@Configuration
@EnableJpaRepositories(“com.acme.repositories”)
class ApplicationConfiguration {

  @Bean
  public EntityManagerFactory entityManagerFactory() {
    // …
  }
}
```

#### 3.5.3. Standalone usage
스프링 컨테이너 바깥에서 repository infrastructure 를 사용할 수 있습니다.

```java
RepositoryFactorySupport factory = … // Instantiate factory here
UserRepository repository = factory.getRepository(UserRepository.class);
```

### 3.6. 스프링 데이터 리파지토리의 커스텀 구현
스프링 데이터 리파지토리는 쉽게 커스텀 리파지토리 코드를 제공하고, 이것을 일반적인 CRUD 추상화와 쿼리 메소드 기능에 통합시키게 해줍니다.

#### 3.6.1. 단일 리파지토리에 커스텀 행동 추가해보기
리파지토리에 커스텀 기능을 좀 더 넣기 위해, 인터페이스를 정의하고, 그 커스텀 기능을 위한 구현체를 정의해야 합니다.

커스텀 리파지토리 기능을 위한 인터페이스
```java
interface UserRepositoryCustom {
  public void someCustomMethod(User user);
}
```

커스텀 리파지토리 기능 구현
```java
class UserRepositoryImpl implements UserRepositoryCustom {

  public void someCustomMethod(User user) {
    // Your custom implementation
  }
}
```

다른 핵심리파지토리 인터페이스와 비교해봤을 때, 클래스에서 가장 중요하게 발견되는 부분은 이름의 `Impl` 접미어입니다.

구현체는 SpringData 에 의존하지 않으며, 정식 Spring Bean 이 될 수 있습니다.

기본 리파지토리 인터페이스를 변경
```java
interface UserRepository extends CrudRepository<User, Long>, UserRepositoryCustom {

  // Declare query methods here
}
```

리파지토리 인프라스트럭쳐는 리파지토리를 발견한 곳에 있는 패키지에서 클래스를 스캔하여 커스텀 구현체를 찾습니다. `repository-impl-postfix` 로 찾는데 기본값은 `Impl` 입니다.

#### 3.6.2. 모든 리파지토리들에 커스텀 행동 추가하기

1. 모든 리파지토리에서 커스텀 행동을 추가하기 위해, 먼저 공유행동으로 선언할 중계인터페이스를 추가합니다.
```java
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable>
  extends PagingAndSortingRepository<T, ID> {

  void sharedCustomMethod(ID id);
}
```

2. 리파지토리 인터페이스는 `Repository` 대신에 이 중계인터페이스를 상속하고 선언된 기능을 포함할 것입니다.

3. 중계 인터페이스의 구현체를 생성합니다.
```java
public class MyRepositoryImpl<T, ID extends Serializable>
  extends SimpleJpaRepository<T, ID> implements MyRepository<T, ID> {

  private final EntityManager entityManager;

  public MyRepositoryImpl(Class<T> domainClass, EntityManager entityManager) {
    super(domainClass, entityManager);

    // Keep the EntityManager around to used from the newly introduced methods.
    this.entityManager = entityManager;
  }

  public void sharedCustomMethod(ID id) {
    // implementation goes here
  }
}
```

4. 커스텀 리파지토리 팩토리를 만들어 기본 `RepositoryFactoryBean` 을 대체하게 합니다. 
```java
public class MyRepositoryFactoryBean<R extends JpaRepository<T, I>, T,
  I extends Serializable> extends JpaRepositoryFactoryBean<R, T, I> {

  protected RepositoryFactorySupport createRepositoryFactory(EntityManager em) {
    return new MyRepositoryFactory(em);
  }

  private static class MyRepositoryFactory<T, I extends Serializable>
    extends JpaRepositoryFactory {

    private final EntityManager em;

    public MyRepositoryFactory(EntityManager em) {

      super(em);
      this.em = em;
    }

    protected Object getTargetRepository(RepositoryMetadata metadata) {
      return new MyRepositoryImpl<T, I>((Class<T>) metadata.getDomainClass(), em);
    }

    protected Class<?> getRepositoryBaseClass(RepositoryMetadata metadata) {
      return MyRepositoryImpl.class;
    }
  }
}
```

5. 커스텀 팩토리를 직접 선언하거나, 스프링 네임스페이스의 `factory-class` 속성을 사용하거나 `@Enable…` 어노테이션으로 repository 인프라스트럭쳐를 설정하여 커스텀 팩토리 구현체를 사용합니다.
```java
@EnableJpaRepositories(factoryClass = "com.acme.MyRepositoryFactoryBean")
class Config {}
```


## JPA 리파지토리
### 4.1. 소개
#### 4.1.1. 스프링 네임스페이스
#### 4.1.2. 어노테이션 기반 설정
`EmbeddedDatabaseBuilder` API 를 사용하여 내장 HSQL 데이터베이스르 절정합니다. `EntityManagerFactory` 를 설정하고 Hibernate 를 샘플 영속 provider 로 사용합니다. 마지막 인프라스트럭쳐 컴포넌트는 `JpaTransactionManager` 로 선언되었습니다.

```java
@Configuration
@EnableJpaRepositories
@EnableTransactionManagement
class ApplicationConfig {

  @Bean
  public DataSource dataSource() {

    EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
    return builder.setType(EmbeddedDatabaseType.HSQL).build();
  }

  @Bean
  public EntityManagerFactory entityManagerFactory() {

    HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
    vendorAdapter.setGenerateDdl(true);

    LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();
    factory.setJpaVendorAdapter(vendorAdapter);
    factory.setPackagesToScan("com.acme.domain");
    factory.setDataSource(dataSource());
    factory.afterPropertiesSet();

    return factory.getObject();
  }

  @Bean
  public PlatformTransactionManager transactionManager() {

    JpaTransactionManager txManager = new JpaTransactionManager();
    txManager.setEntityManagerFactory(entityManagerFactory());
    return txManager;
  }
}
```

### 4.2. 엔티티들 영속화하기 (Persisting)
#### 4.2.1. 엔티티들 저장하기 (Saving)
엔티티를 저장하는 것은 `CrudRepository.save(…)` 메소드를 통해서 이뤄질 수 있습니다. 

### 4.3. 쿼리 메소드들
#### 4.3.1. 쿼리 탐색 전략
JPA 모듈은 쿼리를 수동으로 문자열이나, 메소드네임에서 유추하는 방식으로 정의하는 것을 지원합니다.

`NamedQuery` 를 사용하거나 `Query` 어노테이션을 붙일 수 도 있습니다. 

#### 4.3.2. 쿼리 생성
메소드 이름에서 예약어들과 속성 이름을 파싱해서 쿼리를 생성합니다.

키워드
| Keyword             | Sample                                                       | JPQL snippet                                                 |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `And`               | `findByLastnameAndFirstname`                                 | `… where x.lastname = ?1 and x.firstname = ?2`               |
| `Or`                | `findByLastnameOrFirstname`                                  | `… where x.lastname = ?1 or x.firstname = ?2`                |
| `Is,Equals`         | `findByFirstname`,`findByFirstnameIs`,`findByFirstnameEquals` | `… where x.firstname = 1?`                                   |
| `Between`           | `findByStartDateBetween`                                     | `… where x.startDate between 1? and ?2`                      |
| `LessThan`          | `findByAgeLessThan`                                          | `… where x.age < ?1`                                         |
| `LessThanEqual`     | `findByAgeLessThanEqual`                                     | `… where x.age ⇐ ?1`                                         |
| `GreaterThan`       | `findByAgeGreaterThan`                                       | `… where x.age > ?1`                                         |
| `GreaterThanEqual`  | `findByAgeGreaterThanEqual`                                  | `… where x.age >= ?1`                                        |
| `After`             | `findByStartDateAfter`                                       | `… where x.startDate > ?1`                                   |
| `Before`            | `findByStartDateBefore`                                      | `… where x.startDate < ?1`                                   |
| `IsNull`            | `findByAgeIsNull`                                            | `… where x.age is null`                                      |
| `IsNotNull,NotNull` | `findByAge(Is)NotNull`                                       | `… where x.age not null`                                     |
| `Like`              | `findByFirstnameLike`                                        | `… where x.firstname like ?1`                                |
| `NotLike`           | `findByFirstnameNotLike`                                     | `… where x.firstname not like ?1`                            |
| `StartingWith`      | `findByFirstnameStartingWith`                                | `… where x.firstname like ?1`(parameter bound with appended `%`) |
| `EndingWith`        | `findByFirstnameEndingWith`                                  | `… where x.firstname like ?1`(parameter bound with prepended `%`) |
| `Containing`        | `findByFirstnameContaining`                                  | `… where x.firstname like ?1`(parameter bound wrapped in `%`) |
| `OrderBy`           | `findByAgeOrderByLastnameDesc`                               | `… where x.age = ?1 order by x.lastname desc`                |
| `Not`               | `findByLastnameNot`                                          | `… where x.lastname <> ?1`                                   |
| `In`                | `findByAgeIn(Collection ages)`                               | `… where x.age in ?1`                                        |
| `NotIn`             | `findByAgeNotIn(Collection age)`                             | `… where x.age not in ?1`                                    |
| `True`              | `findByActiveTrue()`                                         | `… where x.active = true`                                    |
| `False`             | `findByActiveFalse()`                                        | `… where x.active = false`                                   |
| `IgnoreCase`        | `findByFirstnameIgnoreCase`                                  | `… where UPPER(x.firstame) = UPPER(?1)`                      |


| Keyword             | Sample                                                       | JPQL snippet                                                 |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `And`               | `findByLastnameAndFirstname`                                 | `… where x.lastname = ?1 and x.firstname = ?2`               |
| `Or`                | `findByLastnameOrFirstname`                                  | `… where x.lastname = ?1 or x.firstname = ?2`                |
| `Is,Equals`         | `findByFirstname`,`findByFirstnameIs`,`findByFirstnameEquals` | `… where x.firstname = 1?`                                   |
| `Between`           | `findByStartDateBetween`                                     | `… where x.startDate between 1? and ?2`                      |
| `LessThan`          | `findByAgeLessThan`                                          | `… where x.age < ?1`                                         |
| `LessThanEqual`     | `findByAgeLessThanEqual`                                     | `… where x.age ⇐ ?1`                                         |
| `GreaterThan`       | `findByAgeGreaterThan`                                       | `… where x.age > ?1`                                         |
| `GreaterThanEqual`  | `findByAgeGreaterThanEqual`                                  | `… where x.age >= ?1`                                        |
| `After`             | `findByStartDateAfter`                                       | `… where x.startDate > ?1`                                   |
| `Before`            | `findByStartDateBefore`                                      | `… where x.startDate < ?1`                                   |
| `IsNull`            | `findByAgeIsNull`                                            | `… where x.age is null`                                      |
| `IsNotNull,NotNull` | `findByAge(Is)NotNull`                                       | `… where x.age not null`                                     |
| `Like`              | `findByFirstnameLike`                                        | `… where x.firstname like ?1`                                |
| `NotLike`           | `findByFirstnameNotLike`                                     | `… where x.firstname not like ?1`                            |
| `StartingWith`      | `findByFirstnameStartingWith`                                | `… where x.firstname like ?1`(parameter bound with appended `%`) |
| `EndingWith`        | `findByFirstnameEndingWith`                                  | `… where x.firstname like ?1`(parameter bound with prepended `%`) |
| `Containing`        | `findByFirstnameContaining`                                  | `… where x.firstname like ?1`(parameter bound wrapped in `%`) |
| `OrderBy`           | `findByAgeOrderByLastnameDesc`                               | `… where x.age = ?1 order by x.lastname desc`                |
| `Not`               | `findByLastnameNot`                                          | `… where x.lastname <> ?1`                                   |
| `In`                | `findByAgeIn(Collection ages)`                               | `… where x.age in ?1`                                        |
| `NotIn`             | `findByAgeNotIn(Collection age)`                             | `… where x.age not in ?1`                                    |
| `True`              | `findByActiveTrue()`                                         | `… where x.active = true`                                    |
| `False`             | `findByActiveFalse()`                                        | `… where x.active = false`                                   |
| `IgnoreCase`        | `findByFirstnameIgnoreCase`                                  | `… where UPPER(x.firstame) = UPPER(?1)`                      |


#### 4.3.2. JPA NamedQuery 사용하기

어노테이션 기반으로 설정
```java
@Entity
@NamedQuery(name = “User.findByEmailAddress”,
  query = “select u from User u where u.emailAddress = ?1”)
public class User {

}
```

Named query 를 실행하기 위해서는 `UserRepository` 에 다음과 같이 명시합니다.

```java
public interface UserRepository extends JpaRepository<User, Long> {

  List<User> findByLastname(String lastname);

  User findByEmailAddress(String emailAddress);
}
```

#### 4.3.4. @Query 사용하기
`@Query` 메소드들을 통해서 도메인 클래스에 어노테이션 하지 않고 직접 바인딩할 수 있습니다.

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query(“select u from User u where u.emailAddress = ?1”)
  User findByEmailAddress(String emailAddress);
}
```

`@Query` 내에서  `LIKE` 표현의 정의를 허용합니다.

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query(“select u from User u where u.firstname like %?1”)
  List<User> findByFirstnameEndsWith(String firstname);
}
```

`LIKE` 구별자 문자 `%` 가 인식이 되면서 유효한 JPQL 쿼리로 변형된느 쿼리가 됩니다.

#### 4.3.5. Using named parameters
`@Param` 어노테이션을 이용해서 메소드 파라미터에 궻적인 이름을 주고 쿼링에 그 이름으로 바인딩시킬 수가 있습니다. 

```java
public interface UserRepository extends JpaRepository<User, Long> {

  @Query(“select u from User u where u.firstname = :firstname or u.lastname = :lastname")
  User findByLastnameOrFirstname(@Param(“lastname”) String lastname,
                                 @Param(“firstname”) String firstname);
}
```

#### 4.3.6. SpEL 표현 사용하기
제한된 SpEL 템플릿 표현을 `@Query` 를 통해서 수동으로 정해진 쿼리에 사용할 수 있습니다.

```java
@Entity
public class User {

  @Id
  @GeneratedValue
  Long id;

  String lastname;
}

public interface UserRepository extends JpaRepository<User,Long> {

  @Query(“select u from #{#entityName} u where u.lastname = ?1")
  List<User> findByLastname(String lastname);
}
```

#### 4.3.7. 쿼리 수정하기 - Modifying queries
쿼리 메소드에 `@Modifying` 로 어노테이션 하면  오직 파라미터 바인딩만을 필요로 하는 수정쿼리의 실행을 할 수 있다.

```java
@Modifying
@Query(“update User u set u.firstname = ?1 where u.lastname = ?2”)
int setFixedFirstnameFor(String firstname, String lastname);
```

이 수정쿼리 호출 시, `EntityManager` 가 자동적응로 cleared 되기를 원한다면 `clearAutomatically` 속성을 true 로 하면 됩니다.

#### 4.4. 스토어 프로시져 - Storedprocedures
`Procedure` 어노테이션을 이용해서 프로시져 메타데이터를 선언할 수 있습니다.

스토어 프로시져를 위한 메타데이터는 엔티티 타입에 `NamedStoredProcedureQuery` 어노테이션을 통해서 설정될 수 있습니다.
```java
@Entity
@NamedStoredProcedureQuery(name = “User.plus1”, procedureName = “plus1inout”, parameters = {
  @StoredProcedureParameter(mode = ParameterMode.IN, name = "arg”, type = Integer.class),
  @StoredProcedureParameter(mode = ParameterMode.OUT, name = “res”, type = Integer.class) })
public class User {}
```

스토어 프로시져는 다양한 방법으로 리파지토리 메소드에 참조될 수 있습니다. 
```java
@Procedure(“plus1inout”)
Integer explicitlyNamedPlus1inout(Integer arg);

@Procedure(procedureName = “plus1inout”)
Integer plus1inout(Integer arg)

@Procedure(name = “User.plus1IO”)
Integer entityAnnotatedCustomNamedProcedurePlus1IO(@Param(“arg”) Integer arg);

@Procedure
Integer plus1(@Param(“arg”) Integer arg);
```


### 4.5. 스페시피케이션 - Specifications
`criteria` 는 쿼리의 where- 구문을 정의합니다. Criteria 는 엔티티에 대해 predicate로 여겨집니다. JPA 는 criteria api 를 사용하는 specification 을 정의하는 api 를 제공합니다.
Specifications 를 지원하기 위해, 리파지토리 인터페이스와 함께 `JpaSpecificationExecutor` 인터페이스를 함께 상속할 수 있습니다.

```java
public interface CustomerRepository extends CrudRepository<Customer, Long>, JpaSpecificationExecutor {
	List<T> findAll(Specification<T> spec);
}
```

`findAll` 메소드는 specification 에 맞는 모든 엔티티를 반환하여 줍니다.

```java
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery<?> query,
            CriteriaBuilder builder);
}
```

매번 필요한 조합을 위한 쿼리를 선언할 필요없이 다음과 같이 사용되고 조합될 수 있습니다.
```java
public class CustomerSpecs {

  public static Specification<Customer> isLongTermCustomer() {
    return new Specification<Customer>() {
      public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query,
            CriteriaBuilder builder) {

         LocalDate date = new LocalDate().minusYears(2);
         return builder.lessThan(root.get(_Customer.createdAt), date);
      }
    };
  }

  public static Specification<Customer> hasSalesOfMoreThan(MontaryAmount value) {
    return new Specification<Customer>() {
      public Predicate toPredicate(Root<T> root, CriteriaQuery<?> query,
            CriteriaBuilder builder) {

         // build query here
      }
    };
  }
}

List<Customer> customers = customerRepository.findAll(isLongTermCustomer());

MonetaryAmount amount = new MonetaryAmount(200.0, Currencies.DOLLAR);
List<Customer> customers = customerRepository.findAll(
  where(isLongTermCustomer()).or(hasSalesOfMoreThan(amount)));
```


### 4.6. Transactionality
리파지토리 인스턴스의 CRUD 메소드들은 기본적으로 transactional 합니다.  읽기 작업을 위해서 트랜잭션 설정 `readOnly` flag 가 true 로 설정되고, 모든 다른 것들은 평문 `@Transactional` 로 설정되어 기본 트랜잭션이 적용되게 됩니다. 

트랜잭션 행동을 변경하는 다른 가능성은 facade 나 일반적으로 하나이상의 리파지토리를 다루는 서비스구현체를 사용하는 것입니다. 그것의 목적은 non-CRUD 작업에 대해 트랜잭션 경계선을 정의하는 데 있습니다.

### 4.7. Locking
사용할 lock 모드를 명시하기 위해 `@Lock` 어노테이션이 쿼리 메소드에서 사용될 수 있습니다.

### 4.8. Auditing
#### 4.8.1. 기초
스프링 데이터는 엔티티를 누가 만들었고 변경했는지, 발생한 시간 지점에 관한 것들을 투명하게 추적하는 세련된 지원을 제공합니다.

어노테이션 기반 auditing 메타데이터
`@CreatedBy`, `@LastModifiedBy` 를 제공하여 누가 엔티티를 생성하고 수정하였는지 뿐만 아니라 `@CreatedDate` , `@LastModifiedDate` 를 사용하여 어떠한 시점에 발생했는지 캡쳐합니다.

인터페이스 기반 auditing 메타데이터
도메인 클래스에 `Auditable` 인터페이스를 구현할 수 있습니다.

AuditorAware
Auditing 인프라스트럭처는 어떻게든 현재 principal 에 대해 알게 될 필요가 있습니다. 그렇게 하기 위해 우리는 `AuditorAware<T> SPI interface` 를 제공하는데 이것은 누가 어플리케이션과 시스템 상호작용을 하는지, 누가 현재 유저인지 인프라 스트럭쳐에 알려주기 위해 구현을 합니다.

```java
class SpringSecurityAuditorAware implements AuditorAware<User> {

  public User getCurrentAuditor() {

    Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

    if (authentication == null || !authentication.isAuthenticated()) {
      return null;
    }

    return ((MyUserDetails) authentication.getPrincipal()).getUser();
  }
}
```

### 4.9. JPA Auditing
#### 4.9.1. 일반적인 auditing 설정
스프링 데이터 JPA 는 엔티티 리스너를 가지고 있습니다. 엔티티리스너는 auditing 정보를 캡쳐하는데 사용될 수가 있습니다. 

```java
@Configuration
@EnableJpaAuditing
class Config {

  @Bean
  public AuditorAware<AuditableUser> auditorProvider() {
    return new AuditorAwareImpl();
  }
}
```




# ref
[Spring-jpa reference](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#project)
[스프링 데이터 JPA 레퍼런스 번역](https://arahansa.github.io/docs_spring/jpa.html)
[git](https://github.com/spring-projects/spring-data-jpa)
[jpa example](https://github.com/spring-projects/spring-data-examples/tree/master/jpa)