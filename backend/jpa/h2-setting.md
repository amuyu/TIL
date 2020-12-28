# Spring 에서 h2 database 사용하기

// memory
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

// server
```yml
spring:
  datasource:
    hikari:
      driver-class-name: org.h2.Driver
#      url: jdbc:h2:mem:ai_doctor
#      url: jdbc:h2:file:./h2/ai_doctor;MODE=mysql
      jdbc-url: jdbc:h2:tcp://localhost:9092/./h2/ai_doctor;MODE=mysql
      username: sa
      password: sa
    initialization-mode: embedded
  h2:
    console:
      enabled: true
      path: /h2-console
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    generate-ddl: true
    hibernate:
      ddl-auto: update
```
config
```java
package com.iconloop.demo.config;

import org.h2.tools.Server;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;
import java.sql.SQLException;

@Configuration
public class H2ServerConfig {

    @Bean
    @ConfigurationProperties("spring.datasource.hikari")
    public DataSource dataSource() throws SQLException {
        Server.createTcpServer("-tcp", "-tcpAllowOthers", "-tcpPort", "9092").start();
        return new com.zaxxer.hikari.HikariDataSource();
    }

}
```
```groovy
// runtimeOnly 에서 compile 로
implementation 'com.h2database:h2'
```