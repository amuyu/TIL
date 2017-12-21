# annotation
## 변수
## target
어노테이션 유형 지정
어노테이션이 필드, 생성자, 파라미터, 메소드 등 어디에서 사용하는지를 표시
## retention
컴파일러 옵션 표시
- RUNTIME : 컴파일 된 클래스 파일에서 유지하다가 클래스가 로딩될 때 이를 읽는다 (retain, reflection)
- CLASS : 컴파일 된 클래스 파일에서 annotation 을 유지하다 런타임 시 무시한다
- SOURCE : 클래스에 포함이 안된다
```java
@Retention(RetentionPolicy.RUNTIME)
```
## Usage
### 메소드
```java
@TestAnno(value="Hello! World!")
public void doing() {
    String methodName = new Throwable().getStackTrace()[0].getMethodName();
    try {
        Method method = getClass().getDeclaredMethod(methodName);
        TestAnno testAnno = method.getAnnotation(TestAnno.class);
        if(testAnno != null)
            System.out.println(testAnno.value());
        else
            System.out.println("null annotation");
    } catch (NoSuchMethodException e) {
        e.printStackTrace();
    }

}
```


## 참고
[Android와 annotation](http://tosslab.github.io/android/2015/03/01/02.android-와-annotation.html)
[자바에서 자신만의 annotation 만들기](http://i5on9i.blogspot.kr/2014/06/annotation.html)
[나만의 어노테이션 만들기](http://gubok.tistory.com/167?srchid=BR1http%3A%2F%2Fgubok.tistory.com%2F167)
[Java 커스텀 Annotation Custom Annotation 만들기](https://medium.com/@ggikko/java-커스텀-annotation-436253f395ad)
[자바 어노테이션](http://jdm.kr/blog/216)
  - 설명이 제일 잘 되어 있음
