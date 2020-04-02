# Build Environment
gradle 은 다음 순서(첫번째가 우선순위가 높음)로 나열된 방법을 사용할 수 있다.
- command line flags 
- System properties : `gradle.properties` 에 저장된 `systemProp.http.proxyHost=somehost.org` 같은 설정
- Gradle properties : `gradle.properties` 에 저장된 `org.gradle.caching=true` 같은 설정
- Environment variables : gradle 을 실행할 때, `GRADLE_OPTS` 같은 환경변수



# ref 
[build environment](https://docs.gradle.org/current/userguide/build_environment.html)