
# Android Log 쉽게 남기기
Android Log를 남길 때, 메시지 출력 위치를 표시하기 위해 tag에는 호출하는 클래스를 입력하고
메시지에는 메소드명을 입력하거나 혹은 같은 메소드에서도 위치를 잘 보기 위해 숫자를 찍는 경우도 있다.(하드코딩)
그런데 안드로이드에서는 메소드를 호출했을 때, 클래스명이나 메소드명 라인 정보를 알려주는 기능을 제공하고 있었다.
그렇다면 클래스, 메소드, 라인 정보를 얻는 방법을 살펴보고 이를 토대로한 샘플 라이브러리도 확인해보자

## StackTraceElement
[StackTraceElement](https://developer.android.com/reference/java/lang/StackTraceElement.html) 클래스는
클래스, 메소드, 라인수등의 정보를 저장하는 역할을 한다. StackTraceElement를 얻기 위해서는
Thread 클래스나 Throwable 클래스를 사용해서 전체 Call Stack 정보를 가져온다.
```java
StackTraceElement[] stackTrace = Thread.currentThread().getStackTrace();
// or
StackTraceElement[] stackTrace = new Throwable().getStackTrace();
```
StackTraceElement[]는 전체 Call Stack 정보를 가지고 있기 때문에 이 중에서 내가 얻고자 하는 정보를
순서에 맞게 잘 뽑아내면 된다. 가장 첫 번째 입력된 정보가 최근에 호출한 Stack 정보이다.
예를 들어, 현재 호출한 Stack 정보는
```java
StackTraceElement[] stacks = ...
StackTraceElement currentStack = stacks[0];
```
바로 이전 Stack 정보는
```java
StackTraceElement[] stacks = ...
StackTraceElement currentStack = stacks[1];
```
이런 식이 된다. 이제 뽑아낸 Stack 정보에서 클래스명, 메소드명, 라인 정보를 가져와보자
### 클래스명
클래스명은 패키지명부터 입력이 된다. 패키지명을 제외한 클래스명만 뽑아내기 위해서는 다음과 같이 호출한다.
```java
StackTraceElement currentStack = ...
String className = currentStack.getClassName();
int indexOfDot = className.lastIndexOf(".");
int indexOfDollar = className.indexOf("$", indexOfDot);
if (indexOfDollar < 0) {
    return className.substring(indexOfDot + 1);
} else {
    return className.substring(indexOfDot + 1, indexOfDollar);
}
```
### 메소드명
메소드명도 마찬기로 메소드명만 뽑아내기 위해서 다음과 같이 호출한다.
```java
StackTraceElement currentStack = ...
String methodName = currentStack.getMethodName();
String[] methodParts = methodName.split("\\$");
for (String methodPart : methodParts) {
    if (!methodPart.equals("lambda")) {
        methodName = methodPart;
        break;
    }
}
```
### 라인
라인은 별다른 필터 없이 그냥 호출해도 된다.
```java
StackTraceElement currentStack = ...
int lineNumber = currentStack.getLineNumber();
```

## Logger(라이브러리)
이제 위의 기능을 사용하면 안드로이드 Log를 편하게 사용할 수 있다.
예를 들어, android.Log 를 사용하면
```java
Log.d("DemoActivity", "onCreate#Activity Created");
```
이렇게 작성해야 하는데, 클래스명과 메소드명을 자동으로 입력하도록 만들면,
다음의 호출만으로도
```java
Logger.d("Activity Created");
```
이런 결과를 얻을 수 있다.
```
...D/DemoActivity#main: onCreate(25) Activity Created
```
이와 관련한 라이브러리 소스는 [Logger](https://github.com/amuyu/Logger) 에서 확인해볼 수 있다.
