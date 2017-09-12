# Android kotlin 변환 중 발생한 realm 관련 에러 - NotFoundException
java 에서 kotlin 으로 변환 중, realm 관련 에러가 발생했다.
`Execution failed for task ':app:transformClassesWithRealmTransformerForDebug'.
> javassist.NotFoundException: pakage.class`

gradle build 중 에러가 발생한다.

이 에러는 java 에서 lambda를 사용하기 위해 추가한 dependency 때문에 발생하는데,
이상한건 빌드 시 에러는 발생하지만 'Run app'을 하면 앱이 정상적으로 실행이 된다..???

어쨋든 빌드 에러를 없애기 위해서 lambda 관련 gradle 설정을 제거한다.
```groovy
// project.gradle
classpath 'me.tatarka:gradle-retrolambda:3.2.2'

// app.gradle
apply plugin: 'me.tatarka.retrolambda'
```
위의 설정 제거!!!

그리고 다시 빌드해서 실행하면 에러가 사라지게 된다.
