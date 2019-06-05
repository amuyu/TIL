# androidTest
## 테스트 유형 및 위치
테스트 코드의 위치는 작성하는 테스트의 유형에 따라 결정됨
- 로컬 단위 테스트 : module-name/src/test/java/
	JVM에서 실행되는 테스트,
- 계측 테스트 : module-name/src/androidTest/java/
	하드웨어 기기나 에뮬레이터에서 실행되는 테스트
	`Instrumentatation` API에 액세스할 수 있음


## Test 클래스 작성
```java
@RunWith(AndroidJUnit4.class)
public class Test {
  ...
  @Test
  public void useAppContext() {
    Context context = InstrumentationRegistry.getTargetContext();
    ...
  }
}
```

## InstantTaskExecutorRule
async 를 동기적으로 실행함

## permission
```kotlin
@get:Rule
    var permissionRule = GrantPermissionRule.grant(android.Manifest.permission.READ_CONTACTS)
```


# 참고
[안드로이드 스튜디오에서 단위 테스트 작성 및 실행하기](http://androidhuman.com/536)
[android app test](https://developer.android.com/studio/test/)
[androidTest - JUnit4, Espresso를 이용한 테스트 코드 작성](https://thdev.tech/androiddev/2016/05/04/Android-Test-Example.html)
[Test apps on Android](https://developer.android.com/training/testing/)
