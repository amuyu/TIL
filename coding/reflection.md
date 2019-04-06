# Reflection
객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
자바클래스가 가진 클래스의 정보를 얻는데 사용
객체의 형에 대해서 모르는 상태에서 객체의 메소드를 호출할 수 있음

# inner class 객체 생성
```java
public class Outer {

    class Inner {
        int age = 10;
    }

    public static void main(String[] args) throws Exception {
        Constructor innerConstructor = Inner.class.getDeclaredConstructor(Outer.class);
        Object outerInstance = Outer.class.newInstance();
        Object newObject = innerConstructor.newInstance(outerInstance);
        Inner inner = (Inner)newObject;
        System.out.println(inner.age);
    }
}
```
http://moigo4.tistory.com/entry/java-reflection-으로-inner-class-객체-생성하기

# innder class check
```java
public static boolean isInnerClass(Class<?> clazz) {
    return clazz.isMemberClass() && !Modifier.isStatic(clazz.getModifiers());
}
```


## 참고
[Java Reflection 개념 및 사용법](http://gyrfalcon.tistory.com/entry/Java-Reflection)
