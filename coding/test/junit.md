JUnit 이란 단위 테스트 도구

## 특징
- 단위 테스트 Framework
- 문자 혹은 gui 기반으로 실행
- 단정문을 테스트 수행 결과를 판별함
- 결과는 성공, 실패 중 하나로 표시

## 단정문
assertArrayEquals(a,b) : 배열 a와b가 일치함을 확인
assertEquals(a,b) : 객체 a와b의 값이 같은지 확인
assertSame(a,b) : 객체 a와b가 같은 객체임을 확인
assertTrue(a) : a가 참인지 확인
assertNotNull(a) : a객체가 null이 아님을 확인

## annotation(JUnit 4)
@Test : test 대상 메소드를 의미
@Test(expected=RuntimeException.class) : RuntimeException 발생해야 성공
@BeforeClass, @AfterClass : 테스트 클래스에서 딱 한번 수행
@Before, @After : 메소드들이 테스트 되기 전과 후에 각각 실행

## JUnit 5
@TestFactory : 동적 테스드들을 위한 테스트 팩토리인 메서드를 나타냅니다.
@DisplayName : 테스트 클래스 나 테스트 메서드을 위한 커스텀  디스플레이 네임을 정의합니다.
@Nested : 이 표시된 클래스는 nested, non-static 테스트 클래스이다 라는 것을 나타냅니다.
@Tag : 필터링 테스트들을 위한 테그들을 정의합니다.
@ExtendWith : custom extensions을 등록할때  사용됩니다.
@BeforeEach : 이 표시된 메서드는 각각의 테스트 메서드 전에 실행됩니다. (이전에는 @Before)
@AfterEach  : 이 표시된 메서드는 각각의 테스트 메서드 후에 실행됩니다. (이전에는 @After)
@BeforeAll : 이 표시된 메서드는 현재 클래스에서 모든 메스트 메서드들 전에  실행됩니다.(이전에는 @BeforeClass)
@AfterAll : 이 표시된 메서드는 현재 클래스에서 모든 테스트 메서드들 후에 실행됩니다.(이전에는 @AfterClass)
@Disable : 테스트 클래스 나 메서드 를 비활성화 할때 사용됩니다.(이전에는 @ignore)


# 참고
[새내기 개발자의 JUnit 여행기](http://www.nextree.co.kr/p11104/)
[juint site](https://junit.org/junit5/)
[Junit 5 소개](http://javacan.tistory.com/entry/JUnit-5-Intro)
[JUnit 5 개념잡기](http://kimseunghyun76.tistory.com/432)
[juint-samples](https://github.com/junit-team/junit5-samples/tree/r5.2.0/junit5-jupiter-starter-gradle)
