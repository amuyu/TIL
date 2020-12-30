# junit no tests found for given includes 에러

intellij 에서 테스트 작성 후, 확인하고자 테스트를 실행했는데 다음과 같은 에러가 발생할 때가 있다.

```
Starting Gradle Daemon...
Gradle Daemon started in 1 s 431 ms

> Task :lib:compileJava UP-TO-DATE
> Task :lib:processResources NO-SOURCE
> Task :lib:classes UP-TO-DATE
> Task :lib:compileTestJava
> Task :lib:processTestResources UP-TO-DATE
> Task :lib:testClasses
> Task :lib:test FAILED
FAILURE: Build failed with an exception.
* What went wrong:
Execution failed for task ':lib:test'.
> No tests found for given includes: [Test.test](filter.includeTestsMatching)
* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.
* Get more help at https://help.gradle.org
BUILD FAILED in 8s
4 actionable tasks: 2 executed, 2 up-to-date
```

음?? 내가 작성한 테스트 코드는 이상이 없는거 같은데 뭐지... 하고 찾아봤더니 
코드에 이상있는 게 아니라 `build.gradle` 에 다음 설정이 누락되어 문제가 발생한 문제였다.

```groovy
test {
    useJUnitPlatform()
}
```

`useJUnitPlatform` 을 따라가보면 다음의 설명이 나온다.

> Specifies that JUnit Platform (a.k.a. JUnit 5) should be used to execute the tests.

테스트 코드에서 `junit 5` 를 사용하는 경우, 위와 같이 설정하면 Junit 5 플랫폼이라는 것을 정의할 수 있다.
위 설정을 추가하고 gradle 설정을 로드한 후, 테스트를 다시 실행하면 정상적으로 실행할 수 있다.

gradle 설정 변경 외에도 실행할 수 있는 방법이 있는데, intellij 설정 화면에서 다음으로 이동한 후,

> Preference > Build,Execution,Deployment > Build Tools > Gradle 

`Run tests using` 항목을 `Gradle(Default)` 에서 `Intellij IDEA` 로 변경한 후, 테스트를 실행해도 위의 에러는 해결 할 수 있다.


# ref
[Intelij 2019.1 update breaks JUnit tests](https://stackoverflow.com/questions/55405441/intelij-2019-1-update-breaks-junit-tests)