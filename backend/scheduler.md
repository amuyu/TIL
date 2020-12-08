
# guide 문서
spring 에서 제공하는 `scheduing-task` 내용 요약

## Starting with spring initializr
`Spring Initializr` 을 사용해서 필요한 dependency 설정을 빠르게 할 수 있다. 
```groovy
plugins {
	id 'org.springframework.boot' version '2.3.2.RELEASE'
	id 'io.spring.dependency-management' version '1.0.8.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}
```

## Adding the `awaitility` dependency
테스트를 위해서 `awaitility` 라이브러리가 필요하다.
```groovy
testImplementation 'org.awaitility:awaitility:3.1.2'
```

## Enable Scheduling
`@EnableScheduling` annotation 을 사용해서 작업이 생성되도록 한다.
```java
package com.example.schedulingtasks;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class SchedulingTasksApplication {

	public static void main(String[] args) {
		SpringApplication.run(SchedulingTasksApplication.class);
	}
}
```

## Create a Scheduled Task
이렇게 하면 scheduled task 를 할 수 있는 설정은 끝났다.
`Scheduled` annotation 을 사용해서 특정 method 를 실행할 수 있다.
```java
/*
 * Copyright 2012-2015 the original author or authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *	  https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.example.schedulingtasks;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ScheduledTasks {

	private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);

	private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

	@Scheduled(fixedRate = 5000)
	public void reportCurrentTime() {
		log.info("The time is now {}", dateFormat.format(new Date()));
	}
}
```
`fixedRate` 는 메소드 호출 시작 ~ 시작 사이의 간격을 지정합니다.
`fixedDelay` 는 메소드 완료~호출 사이의 간격을 지정합니다. 



## ref
[guide](https://spring.io/guides/gs/scheduling-tasks/)
[example](https://github.com/spring-guides/gs-scheduling-tasks)