## gradle ndk compile 해제하기
이클립스에서 사용하던 프로젝트를 안드로이드 스튜디오에서 작업하려는데
gradle 에서 ndk compile 을 진행하면서 에러가 발생해서 빌드가 안된다.
빌드할 때, 라이브러리에서 native까지 빌드를 하지 않아도 상관 없다면 다음과 같이 셋팅할 수 있다.
```groovy
android {
  sourceSets {
        main {
            jni.srcDirs = []
        }
    }
}
```
프로젝트를 빌드할 때, jni가 있으면 자동으로 ndkcompile task 를 실행하게 되는데
jni.srcDirs 를 비워주어 자동으로 ndkcompile task 실행하는 것을 해제할 수 있다.

## 참고
[stackoverflow](https://stackoverflow.com/a/40400058/6759520)
