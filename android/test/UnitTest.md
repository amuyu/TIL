# 안드로이드 테스트 이해
## 안드로이드 테스트 종류
Unit테스트와 UI 테스트로 분류할 수 있다.
### Unit테스트
코드의 유닛 단위(메소드, 클래스, 컴포넌트)의 기능을 실행하는 방식
- 관련 툴 : JUnit, Mockito, PowerMock  

### UI테스트
사용자 인터랙션(버튼클릭, 텍스트 입력 등)을 평가
- 관련 툴 : Espresso, UIAutomator, Robotium, Appium, Calabash, Robolectric

## 자동화 테스트를 하기 위해 뭐가 필요한가요?
특정 패턴의 아키텍쳐를 따라야 한다.
예를 들면, 뷰를 위해 MVP 패턴을, 네트워킹과 데이터 접근을 위해 Repository 패턴을 구현해 테스트할 수 있는 구조를 갖춘다.

## Stub과 driver
driver는 무언가를 이끌고 나가는 것 stub은 담배꽁초나 티켓의 나머지 같은 부분
### 예시
DVD 플레이어 컨트롤러
컨트롤러에서 해당 기능에 대한 로직이 수행되는 것을 테스트 하고 싶은데, 버튼이 아직 없으니까 버튼이 눌려진 것처럼 흉내내는 프로그램을 driver
컨트롤러에서 DVD 장치를 실제로 돌리게 하는 코드를 테스트 하고 싶은데, 실제 장치가 없으니까 장치인척 속여서, 명령을 받고 성공이나 실패 리턴값을 주는 프로그램을 stub



# 기본기
간단한 Junit과 Android Test code 작성
## 셋팅
### build.gradle
```gradle
testInstrumentationRunner "android.test.InstrumentationTestRunner"
```
## method
### assertEquals(a,b)
객체 a,b의 값이 같은지 확인
### assertSame(a,b)
객체 a,b가 같은 객체인지 확인
### assertTrue(a)
a가 참인지 확인
### assertNotNull(a)
a객체가 null이 아님을 확인
### assertArrayEquals(a,b)
배열 a와 b가 일치함을 확인


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
## 이해
### 왜 Mockito가 좋은가? 
#### Stub-run-assert
아래와 같은 코드를 테스트 한다고 하자
```java
public void deleteByHeadline(String headline) {
    // asking/묻기
    Article article = repository.findArticle(headline); // 헤드라인 문자열에 따라 Article 객체 검색
    // telling/시키기
    repository.delete(article); // 해당 객체 삭제를 명령함
}
```
모의 객체 프레임워크가 없다면 우리는 ReporitoryStub 이라는 것을 만들어서 다음과 비슷하게 구현할 것이다.
```java
Article article = new Article();
repositoryStub.setArticleToFind(article); // 스텁에서 리턴해줄 객체를 설정한다.
articleManager.deleteByHeadline("foo"); // 테스트 대상 코드를 실행한다.
repositoryStub.verifyArticleDeleted(article); // 테스트 대상 코드가 삭제코드를 호출했는지 여부를 확인하다.
```
테스트 대상 코드를 호출하기 전에 미리 어떤 값을 리턴할지 지정한(stubbing) 코드를 구현해놓고 테스트 코드를 실행하고 객체에 코드를 삭제했는지 검증한다. 이러한 패턴의 테스트를 stub-run-assert라고 한다.  

### Expect-Run-Verify
Mockito 제작자의 모의 객체 프레임워크는 stub-run-assert 가 아니라 expect-run-verify 형태이다. 간단히 EsayMock 모의 객체 프레임워크로 작성한 코드를 보자
```java
// expect
Article article = new Article();
expect(mockRepository.findArticle("foo")).andReturn(article);
mockRepository.delete(article);

// run
replay(mockRepository);
articleManager.deleteByHeadline("foo");
// verify
verify(mockRepository);
```
여기서 보면 검증해야 할 코드 mockRepository.delete(article)이 테스트 실행 부분보다 위에 있는데 TDD를 해보면 알겠지만, 이는 사고의 흐름에 역행한다.
또 다른 문제도 있는데, 위 코드에서는 mockRepository.findArticle("foo")도 검증 대상이라는 점이다. 이 코드는 분명 스텁인데도 불구하고 verify(mockRepository)가 호출되는 순간 검증 대상에 포함돼 버린다.

### Mockito
Mockito는 가장 자연스러운 흐름에 따라 테스트 코드를 작성할 수 있게 해준다.
```java
Article article = new Article();
// stub
when(mockRepository.findArticle("foo")).thenReturn(article);
// run
articleManager.deleteByHeadline("foo");
// assert
verify(mockRepository).delete(article);
```
스텁을 수작업으로 만든 맨 처음 테스트 코드와 형태가 동일하다. 코드도 간단하다.
사고의 흐름에 역행하지 않고 실제 작성하는 코드의 양도 적다.
클래스와 인터페이스에 대한 모의 객체 생성 문법이 동일하여 구분할 필요가 없고,
hamcrest Matcher도 사용할 수 있다.
[참고](http://kwon37xi.egloos.com/4165915)

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
## Method
### mock()
mock 객체를 만들어서 반환한다.
```java
public class Person {
    private String name;
    private int age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

// mock으로 객체 생성
Person p = mock(Person.class);
assertTrue( p != null );
```
### when()
객체로부터 특정 조건을 지정한다
```java
Person p = mock(Person.class);
when(p.getName()).thenReturn("JDM");
when(p.getAge()).thenReturn(20);
assertTrue("JDM".equals(p.getName()));
assertTrue(20 == p.getAge());
```
지정 메소드에 대해 반환해줄 값을 설정할 수 있다.
```java
public List<스트링> getList(String name, int age){ // do something code }
// list에 대한 호출
when(mockIns.getList(anyString(), anyInt()))
    .thenReturn(
        new ArrayList<스트링>(){
            { this.add("JDM"); this.add("BLOG"); }
        }
    );
    
// 매개 변수에 특정 값을 넣어야 한다면 eq()를 사용
when(mockIns.getList(eq("JDM"), anyInt()))
```
### doThrow()
예외를 던지고 싶을 때, doThrow() 사용
```java
Person p = mock(Person.class);
doThrow(new IllegalArgumentException()).when(p).setName(eq("JDM"));
String name = "JDM";
p.setName(name);
```
### doNothing()
void로 선언된 메소드에 when()을 걸 때
```java
Person p = mock(Person.class);
doNothing().when(p).setAge(anyInt());
p.setAge(20);
verify(p).setAge(anyInt());
```
### verify()
Mock객체의 특정 메서드가 호출되었는 지 확인
```java
Person p = mock(Person.class);
String name = "JDM";
p.setName(name);
// n번 호출했는지 체크
verify(p, times(1)).setName(any(String.class)); // success
// 호출 안했는지 체크
verify(p, never()).getName(); // success
verify(p, never()).setName(eq("ETC")); // success
verify(p, never()).setName(eq("JDM")); // fail
// 최소한 1번 이상 호출했는지 체크
verify(p, atLeastOnce()).setName(any(String.class)); // success
// 2번 이하 호출 했는지 체크
verify(p, atMost(2)).setName(any(String.class)); // success
// 2번 이상 호출 했는지 체크
verify(p, atLeast(2)).setName(any(String.class)); // fail
// 지정된 시간(millis)안으로 메소드를 호출 했는지 체크
verify(p, timeout(100)).setName(any(String.class)); // success
// 지정된 시간(millis)안으로 1번 이상 메소드를 호출 했는지 체크
verify(p, timeout(100).atLeast(1)).setName(any(String.class)); // success
```
### @InjectMocks
클래스 내부에 다른 클래스를 포함하는 경우 사용
@Mock나 @Spy 어노테이션이 붙은 목 객체를 자신의 멤버 클래스와 일치하면 주입시킨다.
```java
public class AuthService{
    private AuthDao dao;
    // some code...
    public boolean isLogin(String id){
        boolean isLogin = dao.isLogin(id);
        if( isLogin ){
            // some code...
        }
        return isLogin;
    }
}
public class AuthDao {
    public boolean isLogin(String id){ //some code ... }
}

// mockito 처리
@Mock
AuthDao dao;

@InjectMocks
AuthService service;

@Test
public void example(){
    MockitoAnnotations.initMocks(this);
    when(dao.isLogin(eq("JDM"))).thenReturn(true);
    assertTrue(service.isLogin("JDM") == true);
    assertTrue(service.isLogin("ETC") == false);
}
```
### @Spy
@Spy로 선언된 목 객체는 목 메소드를 별도로 만들지 않는다면 실제 메소드가 호출된다
```java
Person p = spy(Person.class);
or
Person p = spy(new Person());
or
@Spy Person p;
```

# Hamcrest
assert()와 같은 matcher를 확장한 라이브러리

# Robolectric
JVM 환경에서 안드로이드 앱 테스트 할 수 있는 프레임웍

# Mockito frame work
## Setup
### Download Mockito
AndroidUnitTest 할 때, mockito-core만 사용하면 에러가 발생한다..
```gradle
dependencies {    
    // Optional -- Mockito framework
    testCompile 'org.mockito:mockito-core:1.10.19'
}
```
아래로 셋팅하면 에러 없이 수행함
```gradle
dependencies {    
    androidTestCompile "com.crittercism.dexmaker:dexmaker-mockito:1.4"
    androidTestCompile "com.crittercism.dexmaker:dexmaker-dx:1.4"
}
```

### 버그
#### Class not found .... Empty Test Suite
처음에 Test 패키지로 작성해서 실행했다가 AndroidTest 패키지로 이동 후, 테스트 실행했을 때 발생
##### 해결
실행하는 Edit Configuration 에 Junit Test 로 등록이 되어있는데 이 부분을 제거하고 다시 실행함
[참고](http://stackoverflow.com/a/30172064/6811452)

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



# 비동기 테스트
콜백을 이용하는 메소드 테스트
DummyCollaborator는 자신의 작업을 수행하기 위해 새로운 스레드를 만들고 종료될 때, 결과를 전달한다.


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



# WebView TestCode 작성하기


# How to Test...
##  드라마앤컴퍼니
Model 은 JUnit, Presenter는 ATSL, View는 ATSL에 더하여 Espresso 사용

## TossLab
- [Test적용현황](http://image.slidesharecdn.com/mvp-atsl-livecoding-160331015440/95/mvp-atsl-live-coding-53-638.jpg?cb=1459389370)

# proguard 설정
```gradle
# Proguard rules that are applied to your test apk/code.
-ignorewarnings

-keepattributes *Annotation*

-dontnote junit.framework.**
-dontnote junit.runner.**

-dontwarn android.test.**
-dontwarn android.support.test.**
-dontwarn org.junit.**
-dontwarn org.hamcrest.**
-dontwarn com.squareup.javawriter.JavaWriter
# Uncomment this if you use Mockito
-dontwarn org.mockito.**
```

# 일단 한 번 해보자
## gradle 설정
해봤는데 실패
```gradle
// app/build.gradle
// default COnfig
testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
// dependency
androidTestCompile 'com.android.support:support-annotations:' + rootProject.ext.supportLibVersion;
androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
androidTestCompile 'com.android.support.test:runner:0.5'
androidTestCompile "com.crittercism.dexmaker:dexmaker-mockito:1.4"
androidTestCompile "com.crittercism.dexmaker:dexmaker-dx:1.4"
```

## 참고
- [안드로이드 스튜디오에서 단위 테스트 작성 및 실행하기](http://androidhuman.com/536)
  - Android Junit 테스트 설명, gradle 테스트 실행 명령
- [Android Studio Tips: Unit Testing 적용하기 (Part 1)](https://www.davidlab.net/ko/tech/android-studio-tips-applying-unit-testing-part1/)
  - Junit 테스트 설명, mock object 사용
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
- [Android Webview TestCode](http://thdev.tech/androiddev/2016/08/16/Android-WebView-TestCode.html)
- [Mockito 매뉴얼 한글화](http://blog.naver.com/PostView.nhn?blogId=kjs077&logNo=10137344512)
- [Building Local Unit Tests - Mockito](https://developer.android.com/training/testing/unit-testing/local-unit-tests.html#build)
- [mockito site](http://site.mockito.org/)
- [mockito 사용법](http://jdm.kr/blog/222)
- [Android Testing Codelab](https://codelabs.developers.google.com/codelabs/android-testing/#5)
- [Android-testing-templates](https://github.com/googlesamples/android-testing-templates/blob/master/AndroidTestingBlueprint/app/proguard-test-rules.pro)