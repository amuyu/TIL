// https://github.com/googlesamples/android-architecture-components

# build error
Configuration on demand is not supported by the current version of the Android Gradle plugin since you are using Gradle version 4.6 or above.
Suggestion: disable configuration on demand by setting org.gradle.configureondemand=false in your gradle.properties file or
use a Gradle version less than 4.6.

# BasicRxJavaSampleKotlin
사용자 이름을 입력받아서 room에 저장

## 구조
persistence, ui, injection

## 구현
dependencies : versions.gradle 가져다쓰기
Inejction, kotlin, viewModel, viewModelFactory, room
test code 작성

## domain
User : name, id
put username
get username
