# 기본기
간단한 Junit과 Android Test code 작성
## 셋팅
### build.gradle
```gradle
testInstrumentationRunner "android.test.InstrumentationTestRunner"
```
## 실행
### 스튜디오 실행
test 패키지 하위의 'java' 폴더를 오른쪽 클릭 후, Run > All Tests 선택
테스트 수행 후, `Run` view 에 결과 표시
### Gradle 실행
Gradle 명령을 실행
실행 후, 결과는 build/reports/{테스트패키지}/connected 폴더에 저장된다.
폴더 안의 index.html 파일을 열면 테스트 보고서를 확인할 수 있다.
```bash
gradlew connectedAndroidTest
```
Local Unit Testing 할 때, 
```bash
gradlew test
```

## Junit Test case
덧셈 결과를 반환하는 클래스 작성
### Adder
```java
package com.androidhuman.example.studiotesting;
 
/**
 * Created by kunny on 4/8/14.
 */
public class Adder {
    public int add(int a, int b){
        return a+b;
    }
}
```
### Test코드
@Before는 Test 시작 전에 수행할 것
@After는 Test 후에 수행해야 할 것 
@Test는 실제 Test를 구현한 Test Method 사용
```java
import junit.framework.TestCase;
 
/**
 * Created by kunny on 4/7/14.
 */
public class BasicTest extends TestCase{
 
    public void testSimple(){
        Adder adder = new Adder();
        assertEquals(5, adder.add(2, 3));
    }
}

// JUnit4
public class AdderTest {

    private Adder adder;

    @Before
    public void setUp() throws Exception {
        adder = new Adder();
    }

    @After
    public void tearDown() throws Exception {

    }

    @Test
    public void testAdd() throws Exception {
        assertThat(adder.add(2,3),is(5));

    }
}
```
## Android Test case
Activity의 TextView 문장 표시 테스트
### Test코드
```java
public class MainActivityTest extends ActivityInstrumentationTestCase2<메인> {
 
    public MainActivityTest(){
        super(MainActivity.class);
    }
 
    public void testHelloString(){
        Activity activity = getActivity();
        TextView tvHello = (TextView)activity.findViewById(android.R.id.text1);
        assertEquals(activity.getText(R.string.hello_world), tvHello.getText().toString());
    }
 
}

// JUnit4 + espresso
@RunWith(AndroidJUnit4.class)
@LargeTest
public class HelloWorldEspressoTest {

    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule(MainActivity.class);

    @Test
    public void listGoesOverTheFold() {
        onView(withText("Hello world!")).check(matches(isDisplayed()));
        onView(withId(R.id.view)).check(matches(isDisplayed()));
        onView(withId(R.id.view)).perform(click());
    }
}


```
# 조금 더 상세
## Unit Testing
Unit Testing 에는 두 가지 종류가 있다.
하나는 Android System과 별개로 동작하는 Local Unit Testing이고,
다른 하나는 Android System과 연관되어 동작하는 Instrumented Unit Testing이다.
### Local Unit Testing
개발자의 System에서 Test가 진행되므로 Test 실행시간이 단축된다.


# Mockito
Mock Object를 생성하여 Abstract/Interface를 테스트 할 수 있다.
when, threnReturn을 통해 실제 가상의 함수를 생성하고, 이를 테스트할 수 있다.
Android System에 간단한 의존성을 가지는 경우 (예:Context 사용) 사용하기에 적합하다
## 사용법
### Runner Class 정의
```java
@RunWith(MockitoJUnitRunner.class)
```
### Mock Object 정의
```java
@Mock
private Calc calc;
```
### Mock Object Instance 생성
```java
MockitoAnnotations.initMocks(calc);
```

# Hamcrest
assert()와 같은 matcher를 확장한 라이브러리

# Robolectric
JVM 환경에서 안드로이드 앱 테스트 할 수 있는 프레임웍

# Espresso
## Setup
### Download Espresso
Open your app's `build.gradle` file. 
Add the following lines inside dependencies
```gradle
androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
androidTestCompile 'com.android.support.test:runner:0.5'
```
### Set the instrumentation runner
Add to the same build.gradle file the following line in `android.defaultConfig`
```gradle
testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
```

## 메소드
@Test할 때, 사용하는 메소드

|API|Description|
|---|---|
|onView|View 처리(TextView, EditText, Button 등)|
|withText|해당 테스트 "Hello world!"를 가지는 View|
|withId|R.id.view에 해당하는 view|
|perform|ViewAction 처리|
|click|아이템 클릭 이벤트|
|check(matches())|ViewAssertion 유효성 체크|
|isDisplayed|화면에 노출상태 가져오는 메소드|  


## Espresso basics
You can still safely operate on views by implementing your own `ViewAction`s and `ViewAssertion`s.

Here's an overview of the main components of Espresso
- Espresso - Entry point to interactions with views(via onView and onData)
- ViewMatchers - onView
- ViewActions - ViewInteraction.perform()
- ViewAssertions - ViewInteraction.check()

Example:
```java
onView(withId(R.id.my_view))      // withId(R.id.my_view) is a ViewMatcher
  .perform(click())               // click() is a ViewAction
  .check(matches(isDisplayed())); // matches(isDisplayed()) is a ViewAssertion
```  

## Finding a view with onView
## Performing an action on a view
## Checking if a view fulfills an assertion
## Fragment Test
```java
//fevi-regacy 
MainActivity activity = activityTestRule.getActivity();
        FragmentManager fragmentManager = activity.getSupportFragmentManager();
        movieListFragment = (MovieListFragment) fragmentManager.findFragmentById(R.id.content_frame);
```
# How to Test...
##  드라마앤컴퍼니
Model 은 JUnit, Presenter는 ATSL, View는 ATSL에 더하여 Espresso 사용

## TossLab
![Test적용현황](http://image.slidesharecdn.com/mvp-atsl-livecoding-160331015440/95/mvp-atsl-live-coding-53-638.jpg?cb=1459389370)

## 참고
- [안드로이드 스튜디오에서 단위 테스트 작성 및 실행하기](http://androidhuman.com/536)
- [Android Studio Tips: Unit Testing 적용하기 (Part 1)](https://www.davidlab.net/ko/tech/android-studio-tips-applying-unit-testing-part1/)
- [Android App과 TDD 밑에 자료 보고난 후 공유](http://blog.benelog.net/3017442)
- [Android App과 TDD-kakao임유진](https://docs.google.com/file/d/0B-SpjDXB7EMTMjFsM0k2YTFDbWM/edit)
- [Android에 테스트 도입하기](http://blog.dramancompany.com/2016/08/%ec%95%88%eb%93%9c%eb%a1%9c%ec%9d%b4%eb%93%9c%ec%97%90-%ed%85%8c%ec%8a%a4%ed%8a%b8-%eb%8f%84%ec%9e%85%ed%95%98%ea%b8%b0/)
- [GDG-ATSL-ON-MVP](https://github.com/ZeroBrain/GDG-ATSL-ON-MVP)
- [Robolectric을 활용한 안드로이드 테스팅](http://www.slideshare.net/deview/5robolectric)
- [androidTest - JUnit4, Espresso를 이용한 테스트 코드 작성](http://thdev.tech/androiddev/2016/05/04/Android-Test-Example.html)
- [Android Testing Support Library](https://google.github.io/android-testing-support-library/)
- [MVP의 각 레이어 테스트하기](https://realm.io/kr/news/gdg-seoul-mvp-test/)
- [MVP와 테스트, 라이브코딩](http://www.slideshare.net/ssuser70b5b8/mvp-atsl-live-coding?ref=http://kunny.github.io/community/2016/04/02/gdg_korea_android_weekly_03_2_4/)
- [Android Testing Support Library](https://google.github.io/android-testing-support-library/)