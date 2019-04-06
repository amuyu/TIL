## buildSrc

buildSrc에 class, task, plugin 을 구현해서 사용할 수 있다??





## build-scan

`build-scan` plugin 을 사용해서 build 하면 build 결과를 깔끔하게 조회할 수 있다.

### usage

build.gradle 에 다음을 설정하고 빌드하면 결과를 [build-scan](https://scans.gradle.com/)에서 조회할 수 있다.

```groovy
plugins {
    `build-scan`
}

buildScan {
    setTermsOfServiceUrl("https://gradle.com/terms-of-service")
    setTermsOfServiceAgree("yes")
}
```





## build-cache

build outputs 을 저장하고 input 이 변하지 않았을 때 output 을 가져오도록 한다. 다시 생성하는 시간을 줄일 수 있다.



### 사용 예

Cache 를 적용하지 않고 빌드한 결과:

BUILD SUCCESSFUL in 1s
8 actionable tasks: 8 executed

Cache 적용 후 빌드한 결과

BUILD SUCCESSFUL in 0s
8 actionable tasks: 5 executed, 3 from cache

8개의 tasks 들 중, cache 를 사용해서 5개만 다시 실행해서 속도가 더 빨라진다.



### enable 방법

gradle.properties 에서 활성화한다.

```
org.gradle.caching=true
```



### setting 방법

local 과 remote 를 이용할 수 있고 각각 다음과 같이 설정할 수 있다.

```
buildCache {
    local {
        isEnabled = false
    }
    local(DirectoryBuildCache::class.java) {
        directory = file("path/to/some/local-cache")
        isEnabled = true
        isPush = true
        removeUnusedEntriesAfterDays = 2
    }
    remote(HttpBuildCache::class.java) {
        url = uri("http://example.com/cache")
        isEnabled = false
        isPush = false
        isAllowUntrustedServer = false
        credentials {
            username = "johndoe"
            password = "secret"
        }
    }
}

```




[kotlin-dsl-sample](https://github.com/gradle/kotlin-dsl.git)
