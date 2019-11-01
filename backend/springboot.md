# jar
bootJar

# buidInfo
springboot 의 buildinfo 사용시, 다음과 같은 에러 발생
```
'org.springframework.boot.info.BuildProperties' that could not be found.
```
이 때 intellij 에서 다음 설정을 해주면 됨
> I just needed to set "Delegate IDE build/run actions to gradle" from within "Build, Execution, Deployment->Build Tools->Gradle" which then got the "bootBuildInfo" task to run when building from the IDE.

https://stackoverflow.com/a/52278286

이 모드에서는 코드 수정 사항이 바로 반영되지 않음
IDE 실행에서 buildinfo 를 적용하기 위해서 `src/main/resource/META-INF/build-info.properties` 를 추가하는 방법이 있음
요런 방법 사용

```groovy
springBoot {
    buildInfo()
}

task buildInfoCopy(type: Copy) {
    from file("build/resources/main/META-INF/build-info.properties")
    into "src/main/resources/META-INF"
}

bootBuildInfo.finalizedBy buildInfoCopy
```


# ref
[spring-boot](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/)
[Boot Spring Boot! 북콘서트](https://gist.github.com/ihoneymon/d87396993d2acf6766fe219178f0099c)
[스프링부트 배포파일 구조](https://java.ihoney.pe.kr/523)