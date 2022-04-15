

test 할 때, 로그 보이게 설정
```
tasks.withType<Test> {
    useJUnitPlatform()
    testLogging {
        showStandardStreams = true
    }
}
```