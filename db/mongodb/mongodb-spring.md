
# Spring 에서 MongoDB 연결하기

# dependency
```
implementation 'org.springframework.boot:spring-boot-starter-data-mongodb'
```

# application properties
```
spring.data.mongodb.uri=
spring.data.mongodb.database=
spring.data.mongodb.username=
spring.data.mongodb.password=
```

# Document 정의
```java
@Document(collection = "customers")
public class Customer {

  @Id
  public String id;

  public String firstName;
  public String lastName;

  public Customer() {}

  public Customer(String firstName, String lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  @Override
  public String toString() {
    return String.format(
        "Customer[id=%s, firstName='%s', lastName='%s']",
        id, firstName, lastName);
  }

}
```

# Repository 생성
```java
public interface CustomerRepository extends MongoRepository<Customer, String> {

  public Customer findByFirstName(String firstName);
  public List<Customer> findByLastName(String lastName);

}
```

# save and find
```java
@SpringBootApplication
public class AccessingDataMongodbApplication implements CommandLineRunner {

  @Autowired
  private CustomerRepository repository;

  public static void main(String[] args) {
    SpringApplication.run(AccessingDataMongodbApplication.class, args);
  }

  @Override
  public void run(String... args) throws Exception {

    repository.deleteAll();

    // save a couple of customers
    repository.save(new Customer("Alice", "Smith"));
    repository.save(new Customer("Bob", "Smith"));

    // fetch all customers
    System.out.println("Customers found with findAll():");
    System.out.println("-------------------------------");
    for (Customer customer : repository.findAll()) {
      System.out.println(customer);
    }
    System.out.println();

    // fetch an individual customer
    System.out.println("Customer found with findByFirstName('Alice'):");
    System.out.println("--------------------------------");
    System.out.println(repository.findByFirstName("Alice"));

    System.out.println("Customers found with findByLastName('Smith'):");
    System.out.println("--------------------------------");
    for (Customer customer : repository.findByLastName("Smith")) {
      System.out.println(customer);
    }

  }

}
```


# ref
[Spring Data MongoDB 시작하기](https://velog.io/@hanblueblue/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B83-Spring-Data-MongoDB-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
[Spring Boot MongoDB 와 연결하기](https://ssoonidev.tistory.com/62)
[accessing-data-mongodb](https://spring.io/guides/gs/accessing-data-mongodb/)
[Spring Data MongoDB Reference Guide](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#reference)
[mogodb-getting-started](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#mongodb-getting-started)