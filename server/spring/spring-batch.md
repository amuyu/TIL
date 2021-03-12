

## 스프링 부트 기본 구조
- Job : 여러 Step 으로 하나의 Job 을 만든다.
- Step : Reader, Processor, Writer


## step 다시 시작
```
@Bean
public Step step1() {
	return this.stepBuilderFactory.get("step1")
				.<String, String>chunk(10)
				.reader(itemReader())
				.writer(itemWriter())
				.allowStartIfComplete(true)
				.build();
}
```
https://docs.spring.io/spring-batch/docs/current/reference/html/step.html#allowStartIfComplete


## 우아한 스프링 배치
### 기본편 
배치 애플리케이션이란?

Spring Batch 와 Quartz
Quartz 는 스케줄링 프레임워크

배치 애플리케이션이 필요한 상황
일정 주기로 실행되어야 할 때,
실시간 처리가 어려운 대량의 데이터를 처리 할 때,

Job / Step / Tasklet


### 활용편

batch-h2.properties
```
# Placeholders batch.*
#    for H2:
batch.jdbc.driver=org.h2.Driver
batch.jdbc.url=jdbc:h2:file:build/data/h2
batch.jdbc.user=sa
batch.jdbc.password=
batch.database.incrementer.class=org.springframework.jdbc.support.incrementer.H2SequenceMaxValueIncrementer
batch.schema.script=classpath:/org/springframework/batch/core/schema-h2.sql
batch.drop.script=classpath:/org/springframework/batch/core/schema-drop-h2.sql
batch.jdbc.testWhileIdle=true
batch.jdbc.validationQuery=


# Non-platform dependent settings that you might like to change
batch.data.source.init=true
batch.table.prefix=BATCH_


```


# ref
[Spring Batch 가이드 - Spring Batch Job Flow](https://jojoldu.tistory.com/328)
[간단한 대용량 배치처리, 스프링부트배치](https://ahndy84.tistory.com/18)
[Asynchronous Processors](https://docs.spring.io/spring-batch/docs/current/reference/html/spring-batch-integration.html#asynchronous-processors)
[우아한 스프링 배치](https://www.youtube.com/watch?v=_nkJkWVH-mo)
[Spring Batch 의 유니크 Job Parameter 활용하기](https://jojoldu.tistory.com/487)
[spring batch database](https://docs.spring.io/spring-boot/docs/2.0.0.M7/reference/htmlsingle/#howto-initialize-a-spring-batch-database)