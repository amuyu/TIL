# Execution failed for task 'app:mergeDebugResources'
이클립스에서 안드로이드 스튜디오로 프로젝트 import 후, 빌드할 때 다음과 같은 에러가 발생할 수 있다.
```sh
Execution failed for task ':app:mergeDebugResources'.
...
libpng error: Not a PNG file
```
이클립스에서 프로젝트 빌드가 정상적으로 됐었는데 리소스 파일들을 하나하나 확인하려니 영 귀찮다.
gradle 에서 위와 같은 에러를 무시하는 옵션을 제공하고 있다.
build.gradle 파일에 다음과 같은 설정을 추가하면 된다.
```groovy
android {
  aaptOptions
  {
      cruncherEnabled = false
  }
}
```
