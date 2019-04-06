# kotlin-dsl
gradle script kotlin
최신 버전 gradle 에서 kotlin-dsl 버전이 포함됨

`build.gradle` 에 확장명을 추가하면 gradle에서 kotlin 파일로 사용할 수 있다. `build.gradle.kts`


터미널을 사용해 컴파일 해야 빌드 실패 원인을 파악할 수 있다.
```
./gradlew assembleDebug --info
```

# buildSrc
폴더에서 `build.gradle.kts` 에 다음 내용을 추가합니다.
```groovy
plugins {
    `kotlin-dsl`
}
```

# 동기화
마지막으로, Gradle 동기화를 수행하려고하면 프로젝트가 새로운 Gradle 빌드 스크립트를 찾지 못해 실패하게됩니다.
build.gradle. 이전 스크립트를 실행할 것으로 예상됩니다 . settings.gradleGradle이 `build.gradle.kts` 파일을
이전 스크립트 대신 빌드 스크립트로 사용하려면 하나의 행을 추가 하기 만하면됩니다 .
settings.gradle 파일을 편집 하고 다음을 추가하십시오
```groovy
rootProject.buildFileName = 'build.gradle.kts'
```


# build.gradle
build.gradle 파일을 조금 남겨놓을 수도 있다.
```groovy
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        classpath(Config.BuildPlugins.androidGradle)
        classpath(Config.BuildPlugins.kotlinGradlePlugin)
    }
}

allprojects {
    repositories {
        jcenter()
        google()
    }
}
```

# compile
```sh
gradle compileKotlin
```

# error?
build.gradle 파일이 없으면 `The project is not a Gradle-based project` 메시지가 출력된다.

`./gradlew assembleDebug --info` 이거는 에러가 안나네 왜그러지?

setting.gradle 파일에
rootProject.buildFileName = 'build.gradle.kts' 해서 되나?

# 참고
[Kotlin DSL to write Gradle scripts on Android: Step by step walkthrough](https://antonioleiva.com/kotlin-dsl-gradle/)
[Gradle Kotlin DSL Tutorial](https://medium.com/@napperley/gradle-kotlin-dsl-tutorial-223370af9cd8)
[Android 용 Gradle Script Kotlin 사용](https://medium.com/@arturogdg/using-gradle-script-kotlin-for-android-d6cd58c80d60)
[kotlin-dsl](https://github.com/gradle/kotlin-dsl/)
[project with buildSrc](https://github.com/gradle/kotlin-dsl/tree/master/samples/project-with-buildSrc)
[project-properties](https://github.com/gradle/kotlin-dsl/tree/master/samples/project-properties)
[gradle guide](https://guides.gradle.org/building-kotlin-jvm-libraries/)
  - 이거 보고 함
