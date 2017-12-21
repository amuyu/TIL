## Guava
Guava는 구글이 작성한 자바 오픈소스 라이브러리
### Guava를 사용하면 좋은 5가지 이유
1. 컬렉션 초기화와 유틸리티
컬렉션 생성을 간단하게 하고 많은 유틸리티 함수가 Maps, Sets등의 컬렉션에 들어가있다.

  ```java
    // java
    final Map<스트링, 맵<스트링, 인티저>> lookup = new HashMap<스트링, 맵<스트링, 인티저>>();

    // guava
    final Map<스트링, Map<스트링, Integer>> lookup = Maps.newHashMap();
  ```  

2. 제한된 함수형 스타일의 프로그래밍
Guava는 함수형 스타일로 메서드를 전달하기 위한 일반적인 메서드를 제공한다.(Collection2.transform)
또한 Collection2는 컬렉션에서 어떤 값을 제한하도록하는 filter 메서드를 가지고 있다.
  ```java
	Collection<?> noNullsCollection = filter(someCollection, notNull());
  ```  

3. 멀티맵(Multimaps과 바이맵(Bimaps)
단일키에 여러 값을 저장하는 것 등은 Map의 정말 일반적인 사용입니다. 일반적으로 표준 자바 컬렉션의 사용은 값타입처럼 또 다른 컬렉션을 사용함으로써 이루어진다. 이것은 컬렉션 초기화같은 반복되는 많은 형식을 포함하게 되는데 멀티맵은 이것을 아주 깔끔하게 만들어준다.
  ```java
  	Multimap<String, Integer> scores = HashMultimap.create();
    scores.put("Bob", 20);
    scores.put("Bob", 10);
    scores.put("Bob", 15);
    System.out.println(Collections.max(scores.get("Bob"))); // prints 20
  ```

4. 쉬운 해쉬코드와 비교자
자바에서 필드들의 해쉬코드로 클래스의 해쉬코드를 생성하는 것은 아주 일반적이다. Guava는 이것을 위해서 Object 클래스에 유틸리티 메서드를 제공한다.
  ```java
    int foo;
    String bar;

    @Override
    public int hashCode() {
      return Objects.hashCode(foo, bar);
    }
  ```
Guava는 비교자(Comparator) 과정을 쉽게 하기 위해 COmparisonChain클래스를 제공한다.
  ```java
    int foo;
    String bar;

    @Override
    public int compareTo(final GuavaExample o) {
      return ComparisonChain.start().compare(foo, o.foo).compare(bar, o.bar).result();
    }
  ```
5. 방어적 코딩
Guava는 Preconditions 클래스를 일반적인 필수조건의 시리즈로 제공한다.
  ```java
  	checkArgument(count > 0, "must be positive: %s", count);
  ```

### Guava 결론
Guava를 사용하면 유지보수를 위해 많은 필요한 양의 코드를 줄이고 생산성을 높여준다.

### 사용예시
hashCode 생성 시, 사용하면 편리하다.
```java
Objects.hashCode(..);
```

## 참고
[Guava로 Ordering으로 컬렉션 정렬하기](https://blog.outsider.ne.kr/715)
