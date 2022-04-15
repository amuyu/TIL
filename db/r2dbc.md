
# r2dbc란?
Reactive relational database connectivity
적은 수의 스레드로 동시성을 처리, 더 적은 하드웨어 리소스롤 확장할 수 있는 non-blocking 애플리케이션 스택

# sql 실행 
* DatabaseClient
* R2dbcEntityTemplate
* R2dbcRepository

# WebFlux 가 뭔데?
reactive

# mono, flux 어떤때?
* Flux : 0 ~ N 개의 데이터 전달
* Mono : 0 ~ 1 개의 데이터 전달

# paging
https://stackoverflow.com/a/63787029
webflux r2dbc 는 안된다? `CoroutineCrudRepository` 

# kotlin
https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#kotlin.coroutines.reactive
* fun handler(): Mono<Void> becomes suspend fun handler()
* fun handler(): Mono<T> becomes suspend fun handler(): T or suspend fun handler(): T? depending on if the Mono can be empty or not (with the advantage of being more statically typed)
* fun handler(): Flux<T> becomes fun handler(): Flow<T>

## flow operatior
https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-core/common/src/flow/operators

# ref
[r2dbc](https://r2dbc.io/)
[spring-r2dbc](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/)
[spring-data-r2dbc](https://brunch.co.kr/@purpledev/28)