

https://github.com/jojoldu/blog-code/blob/master/spring-boot-entityql/build.gradle
```groovy
configurations {
        all {
            resolutionStrategy {

                // 특정 모듈의 버전을 강제 지정(최상위건 이행적 의존성이건 무관함)
                force  'com.google.guava:guava:20.0'
            }
        }
    }
```