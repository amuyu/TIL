Mockito 자바에서 단위 테스트를 하기 위해 Mock 을 만들어주는 프레임워크

# 간단한 사용법
stub, use, verify
mock(), spy() 을 사용해서 Mock을 생성할 수 있다.
- Mockito.when(mock.action()).thenReturn(true)
- BDDMockito.given(mock.action()).willReturn(true)
제대로 동작하는지 verification 할 수 있다.
- Mockito.verify(mock).action()
- BDDMockito.then(mock).should().action()
@Mock, @Spy, @Captor or @InjectMocks과 같은 Annotation을 사용할 수 있습니다.

# Detail
## 동작 검증
```java
// 코드를 깔끔하게 만들기 위해 Mockito를 static 으로 import 하자.
import static org.mockito.Mockito.*;

// mock 생성
List mockedList = mock(List.class);

// mock 객체 사용
mockedList.add(“one”);
mockedList.add(“two”);

// 검증 하기
verify(mockedList).add(“one”);
verify(mockedList).clear();
```

## stubbing
```java
// interface 뿐 아니라 구체 클래스도 mock으로 만들 수 있다.
LinkedList mockedList = mock(LinkedList.class);

// stubbing
when(mockedList.get(0)).thenReturn("first");
when(mockedList.get(1)).thenThrow(new RuntimeException());

// 첫 번째 element를 출력한다.
System.out.println(mockedList.get(0));

// runtime exception이 발생한다.
System.out.println(mockedList.get(1));

// 999번째 element 얻어오는 부분은 stub되지 않았으므로 null이 출력
System.out.println(mockedList.get(999));

// stubbing 된 부분이 호출되는지 확인할 수 있긴 하지만 불필요한 일입니다.
// 만일 코드에서 get(0)의 리턴값을 확인하려고 하면, 다른 어딘가에서 테스트가 깨집니다.
// 만일 코드에서 get(0)의 리턴값에 대해 관심이 없다면, stubbing되지 않았어야 합니다. 더 많은 정보를 위해서는 여기를 읽어보세요.
verify(mockedList).get(0)
```

## Argument matchers
```java
// 내장된 argument matcher인 anyInt()를 이용한 stubbing
when(mockedList.get(anyInt())).thenReturn("element");

// hamcrest를 이용한 stubbing (isValid()는 사용자가 정의한 hamcrest matcher이다.)
when(mockedList.contains(argThat(isValid()))).thenReturn("element");

// 다음 코드는 "element"를 출력한다.
System.out.println(mockedList.get(999));

// argument matcher를 이용해 검증할 수도 있다.
verify(mockedList).get(anyInt());
```

## 몇 번 호출 됐는지
```java
// mock 설정
mockedList.add("once");

mockedList.add("twice");
mockedList.add("twice");

mockedList.add("three times");
mockedList.add("three times");
mockedList.add("three times");

// 아래의 두 가지 검증 방법은 동일하다. times(1)은 기본값이라 생략되도 상관없다.
verify(mockedList).add("once");
verify(mockedList, times(1)).add("once");

// 정확히 지정된 횟수만큼만 호출되는지 검사한다.
verify(mockedList, times(2)).add("twice");
verify(mockedList, times(3)).add("three times");

// never()를 이용하여 검증한다. never()는 times(0)과 같은 의미이다.
verify(mockedList, never()).add("never happened");

// atLeast()와 atMost()를 이용해 검증한다.
verify(mockedList, atLeastOnce()).add("three times");
verify(mockedList, atLeast(2)).add("five times");
verify(mockedList, atMost(5)).add("three times");
```

## 순서 검증하기
```java
List firstMock = mock(List.class);
List secondMock = mock(List.class);

// mock을 사용한다.
firstMock.add("was called first");
secondMock.add("was called second");

// mock이 순서대로 실행되는지 확인하기 위해 inOrder 객체에 mock을 전달한다.
InOrder inOrder = inOrder(firstMock, secondMock);

// 다음 코드는 firstMock이 secondMock 보다 먼저 실행되는 것을 확인한다.
inOrder.verify(firstMock).add("was called first");
inOrder.verify(secondMock).add("was called second");
```

## 실행되지 않았는지 확인하기
```java
// mock 사용하기 - mockOne만 호출된다.
mockOne.add("one");

// 일반적인 검증
verify(mockOne).add("one");

// 메소드가 호출되지 않았는지 검증
verify(mockOne, never()).add("two");

// 다른 mock이 호출되지 않았는지 검증
verifyZeroInteractions(mockTwo, mockThree);
```

## 불필요하게 실행되는 코드 찾아내기
```java
// mock 설정
mockedList.add("one");
mockedList.add("two");

verify(mockedList).add("one");

// 다음 구문은 실패한다.
verifyNoMoreInteractions(mockedList);
```

## 간단하게 mock 생성하기
```java
public class ArticleManagerTest {
@Mock private ArticleCalculator calculator;
@Mock private ArticleDatabase database;
@Mock private UserProvider userProvider;

private ArticleManager manager;
}
// ...
```

## 연속적인 콜 stubbing 하기
```java
when(mock.someMethod("some arg"))
    .thenThrow(new RuntimeException())
    .thenReturn("foo");

// 첫 번째 호출 : runtime exception을 던져라.
mock.someMethod("some arg");

// 두 번째 호출 : "foo"를 출력해라.
System.out.println(mock.someMethod("some arg"));

// 그 다음에 일어나는 모든 호출 : "foo"를 출력한다. (마지막 stubbing이 계속 발생한다.)
System.out.println(mock.someMethod("some arg"));

// 연속적인 호출에 대한 stubbing을 간략화할 수 있다.

when(mock.someMethod("some arg")).thenReturn("one", "two", "three");
```

## 실제 객체 감시하기
- 실제 객체에 대해 when(Object) 를 사용할 수 없습니다.
- final method 는 mock으로 만들지 않습니다.
```java
List list = new LinkedList();
List spy = spy(list);
// 특정 메소드만 stub 하는 것이 가능하다.
when(spy.size()).thenReturn(100);

// 스파이를 이용해 real method를 호출하기
spy.add("one");
spy.add("two");

// 리스트의 첫 번째 element인 "one"을 출력해라
System.out.println(spy.get(0));

// size() 가 stub 되었으므로 100이 출력된다.
System.out.println(spy.size());

// 검증도 가능
verify(spy).add("one");
verify(spy).add("two");
```

## 파라미터 검증하기
```java
ArgumentCaptor<Person> argument = ArgumentCaptor.forClass(Person.class);
verify(mock).doSomething(argument.capture());
assertEquals("John", argument.getValue().getName());
```

## 새로운 어노테이션 (>=1.8.3)
@Captor 는 ArgumentCaptor 생성을 간략화 시켰습니다.
@Spy - spy(Object)대신에 사용할 수 있습니다.
@InjectMocks - 테스트 될 객체에 mock을 자동으로 넣어줍니다.


# 참고
[mockito-features](https://github.com/mockito/mockito/wiki/Mockito-features-in-Korean)
