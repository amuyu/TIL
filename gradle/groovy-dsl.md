# Groovy DSL
Domain Specific Language?
여기서는 빌드 자동화에 최적화된 언어를 말하는 듯 하다.
Gradle 이라는 도메인에 최적화된 언어

# basic
Gradle script 는 configuration script 이다.
스크립트가 실행될 때 특정 유형의 object 를 configure 한다.
예를 들어 Build script 를 실행할 때, `Project` object 를 configure 한다.
object 들을 delegate object 라고 부른다.

- Build script -> Project
- Init script -> Gradle
- Settings script -> Settings

각각의 script 들은 delegate object 의 메소드와 properties 를 사용할 수 있다.

Gradle script 는 `Script`interface 를 구현해서 여러가지 properties 와 Methods 를 정의할 수 있다.

# Build script
allprojects {}
buildscript {}
configurations {}

# Core types
gradle scripts 에서 주로 사용하는 type
Project
Task
Gradle

# ref
[Gradle Build Language Reference](https://docs.gradle.org/current/dsl/index.html)