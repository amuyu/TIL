# androidTest
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

# 참고
[안드로이드 스튜디오에서 단위 테스트 작성 및 실행하기](http://androidhuman.com/536)
