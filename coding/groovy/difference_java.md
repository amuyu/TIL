### 자바와의 차이점

1. Default imports

다음의 패키지들이 default 로 import 되어 있다.


- java.io.*
- java.lang.*
- java.math.BigDecimal
- java.math.BigInteger
- java.net.*
- java.util.*
- groovy.lang.*
- groovy.util.*

2. Multi-methods

메소드를 runtime 시에 선택해서 invoke 한다. 자바에서는 컴파일 시에 선언한 타입을 보고 선택한다.

```java
int method(String arg) {
    return 1;
}
int method(Object arg) {
    return 2;
}
Object o = "Object";
int result = method(o);
```

자바에서는 result 가 2가 된고 groovy 에서는 result 가 1이 된다.

3. Array initializers

`{...}` 가 closure 로 예약되어 있어서 array 초기화에 사용할 수 없다.

```groovy
int[] array = { 1, 2, 3} // not use
int[] array = [1,2,3] // have to use
```

4. Package scope visibility

field 의 modifier 를 제거해서 자바에서와 동일하게 package-private 를 사용할 수 없다. `@PackageScope` 를 사용해서 package-private field 를 만들 수 있다.

```groovy
// 자바에서 name 은 package-private
class Person {
    String name
}
// groovy
class Person {
    @PackageScope String name
}
```

5. ARM blocks

java7 부터 사용가능한 ARM 은 groovy 에서는 지원하지 않는다.

```java
// ARM
Path file = Paths.get("/path/to/file");
Charset charset = Charset.forName("UTF-8");
try (BufferedReader reader = Files.newBufferedReader(file, charset)) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }

} catch (IOException e) {
    e.printStackTrace();
}
```

이를 구현하려면 다음과 같이 한다.

```groovy
new File('/path/to/file').eachLine('UTF-8') {
   println it
}
// use closer
new File('/path/to/file').withReader('UTF-8') { reader ->
   reader.eachLine {
       println it
   }
}
```

6. Inner classes

inner class 는 자바에 따르지만 차이가 있다.

6.1. Static inner classes

```groovy
class A {
    static class B {}
}

new A.B()
```

6.2. Anonymouse Inner Classes

```groovy
import java.util.concurrent.CountDownLatch
import java.util.concurrent.TimeUnit

CountDownLatch called = new CountDownLatch(1)

Timer timer = new Timer()
timer.schedule(new TimerTask() {
    void run() {
        called.countDown()
    }
}, 0)

assert called.await(10, TimeUnit.SECONDS)
```

6.3. Creating Instances of Non-static Inner Classes

```groovy
public class Y {
    public class X {}
    public X foo() {
        return new X()
    }
    public static X createX(Y y) {
        return new X(y)
    }
}
```

7. Lambdas

lambda 대신에 closures 를 사용한다.

```groovy
// java
Runnable run = () -> System.out.println("Run");
list.forEach(System.out::println);
// groovy
Runnable run = { println 'run' }
list.each { println it } // or list.each(this.&println)
```

8. String

single-quote 는 `String` 이다.

double-quote string 은 `GString` 와 `String` 으로 cast 한다.

```groovy
assert 'c'.getClass()==String
assert "c".getClass()==String
assert "c${1}".getClass() in GString
```

`char` 는 type 을 지정해서 사용한다.

```groovy
char a='a'
assert Character.digit(a, 16)==10 : 'But Groovy does boxing'
assert Character.digit((char) 'a', 16)==10

try {
  assert Character.digit('a', 16)==10
  assert false: 'Need explicit cast'
} catch(MissingMethodException e) {
}
```

10. Primitives and wrappers

groovy 는 모두 객체로 사용하기 때문에 primitives type 을 autowrap 한다.

```groovy
int i
m(i)
// 자바에서 호출하는 함수
void m(long l) {           
  println "in m(long)"
}
// groovy에서 호출하는 함수
void m(Integer i) {        
  println "in m(Integer)"
}
```

11. `==` 동작

groovy 에서 `==` 는 Comparable 이면  `a.compareTo(b) == 0` 과 같이 변환하고 그렇지 않으면 a.equals(b) 로 변환한다. java 의 의미로 사용하려면 `is` 를 사용해서  `a.is(b)`  같이 쓴다.

12. Extra keywords

as, def, in, trait 가 있다.
