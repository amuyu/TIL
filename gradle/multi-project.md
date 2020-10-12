
## multi-project build 하기
intellij gradle 환경에서 multi project 를 build 하는 방법


1. basic type 으로 gradle project 를 만든다. (framework 없이)
empty project 로 생성한다.
framework 을 지정하게 되면 rootProject 가 특정 framework 의 속성을 갖게 되고 관련된 폴더들이 자동으로 생성된다. 예를 들어, java 를 선택하게 되면 java 관련 설정들과 폴더들이 자동으로 생성된다. `src/main/java` 같은 ..
2. sub module 을 추가한다.
이 때, 각각 module 이 요구하는 framework 을 선택한다.
3. root/build.gradle 설정할거?
buildscript
allprojects
일단 세부적인 설정은 `module/build.gradle` 에서 설정하고 rootProject 는 공통적으로 잡아야할 설정을 넣는다. 처음에는 딱히 설정하게 없다.
```groovy
allprojects {
    group = 'org.gradle.sample'
    version = '1.2'
}
```


### ref
[gradle 에서 multi 프로젝트 만들기](http://yookeun.github.io/java/2017/10/07/gradle-multi/)
https://docs.gradle.org/current/samples/sample_jvm_multi_project_build.html