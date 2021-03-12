# Gradle jar 빌드하기 전 test 실행
`./gradlew jar` 혹은 `Intellij IDEA` 의 `Gradle Tool Window` 에서 `tasks > build > jar` 로 jar 파일을 빌드할 때,
`jar` task 를 실행하기 전, 항상 `test` 를 실행하도록 설정하는 방법입니다.

물론 `test` task 를 실행하고 `jar` task 를 실행하면 되지만 혹시나
```shell
./gradlew clean test jar
```

예를 들어, 다음과 같은 설정이 있습니다. 
build.gradle
```
jar {
    from configurations.compile.collect { it.isDirectory() ? it : zipTree(it) }
    finalizedBy shadowJar
}
```
task 의 dependency 를 설정하는 `dependsOn` 을 사용해서 다음과 같이 변경 가능합니다.
```
jar {
    dependsOn 'test'
    from configurations.compile.collect { it.isDirectory() ? it : zipTree(it) }
    finalizedBy shadowJar
}
```
아무것도 없는 경우, `dependsOn` 설정만 추가 작성합니다.
```
jar {
    dependsOn 'test'
}



## ref
[Gradle - Create jar only if tests pass](https://stackoverflow.com/a/38641491)