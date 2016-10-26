# TIL
# 업무
## 개발 프로세스
[토스랩, 이렇게 일하고 있습니다.](http://tosslab.github.io/process/2016/02/25/how-to-work-in-tosslab.html)

## 코드리뷰
### 요청 방법
- Title
  - Featur/Bug-fix 건
  - 어떤 목적
- 어떤 이슈와 연결됐는지
- Description
  - 어떤 로직을 추가/수정했는지
  - 어떻게 추가 수정했는지


### 코드리뷰는 언제?
코드리뷰가 요청이 오면 업무의 최우선순위로 조정되어 즉시 응답하도록 하는 것이 원칙이다.
(지금 당장 하든지 아니면 언제부터 할 것인지를 피드백을 반드시 준다.)
이를 바탕으로 현실적으로 수정
1. 월~수 : feature/bug-fix 개발이 업무의 최우선 순위
2. 목,금 : 코드리뷰가 업무의 최우선 순위이며 코드리뷰 대상은 목요일 출근 전까지 리뷰 요청한 건

### 무엇을 리뷰하나요?
- 성능 개선 개발 : 시간복잡도
- 신규 feature 개발 : 잠재적인 오류 검출
- 리팩토링 : 테스트코드나 구조에 대한 물음
- 신규 기술 도입 : 해당 기술의 로직과 그에 대한 물음
- 기타 : 변수명과 같은 코드 컨벤션, 실제 빌드 해서 동작

### 코드리뷰 코멘트는 어떻게
- oo보다는 xx가 더 나은 것 같아요
- xx는 oo부분을 참고해서 이용하면 되요
- oo는 xx에 의해서 문제되지 않을까요?
- xx를 하려다가 oo로 했는데 어떻게 생각하세요?

### 코드리뷰가 끝나면 어떻게
리뷰가 완료되면 개발용 브랜치에 통합한다. 최소 1명의 피드백도 진행되지 않은 코드는 통합하지 않는다.

### 긴급히 코드리뷰해야 하는 건은 어떻게 하나요?
즉시 진행한다.

### 코드 리뷰 시스템
"p4 mail"이나 gerrit 같은 코드 리뷰 시스템을 사용하는 것도 좋은 듯..
온라인 상에서의 코드 리뷰

### 코드 리뷰에 대한 인식, 태도, 문화?
코드 리뷰를 통해서 많이 배운다. 나보다 더 나은 엔지니어로부터 조언을 받을 수 있으니까
-> 실력이 좋은 사람으로부터 배워야 한다!!!

### 참고
[코드리뷰, 이렇게 하고 있습니다.](http://tosslab.github.io/codereview/2015/12/18/%EC%BD%94%EB%93%9C%EB%A6%AC%EB%B7%B0-%EC%9D%B4%EB%A0%87%EA%B2%8C-%ED%95%98%EA%B3%A0-%EC%9E%88%EB%8B%A4.html)
[코드 리뷰 어떻게 하나요?](http://sv-story.blogspot.kr/2013/04/blog-post_28.html)


# 코딩
## Builder Pattern
객체 생성 시, Builder 패턴을 사용하면 파라미터가 많을 경우 제공 상태를 일관성 있게 하고, object를 생성시킬 때, step-by-step으로 만들 수 있도록 할 수 있다.
### 작업순서
1) class 안에 중첩 static class를 생성시키고, 바깥쪽 class의 argument들을 안쪽 static class(builder class)로 옮긴다. 바깥쪽 class의 생성자는 private로 선언해서 직접적인 생성을 막는다.
2) builder class의 생성자를 public static으로 선언하고 필요한 파라메터들을 요청한다.
3) builder class에는 선택적 파라메터에 대한 setter method가 있어야한다. 그리고 선택적 인자를 설정한 후에도 같은 builder object를 리턴해야한다.
4) 마지막으로로 클라이언트 프로그램이 요청하는 object를 받을 수 있도록 build method를 만드는데, build method에서는 바깥쪽 class의 생성자가 builder 클래스의 인자를 받을 수 있도록 제공한다.

## 리액티브 프로그래밍
뭐지?

# 안드로이드
## Gradle Dependency 분리하기
라이브러리의 이름과 버전을 분리 별도로 분리하여 관리한다.
### 참고
[Gradle Dependency 분리하기](http://tosslab.github.io/android/2016/10/10/dependencies-of-gradle.html)


## key-value xml
```xml
<string-array name="my_array">
    <item>key1|value1</item>
    <item>key2|value2</item>
</string-array>
```
```java
Map<String, String> getKeyValueFromStringArray(Context ctx) {
int id = context.getResources().getIdentifier(resourcename, "array", context.getPackageName());
    String[] array = ctx.getResources().getStringArray(id);
    Map<String, String> result = new HashMap<>();
    for (String str : array) {
        String[] splittedItem = str.split("\\|");
        result.put(splittedItem[0], splittedItem[1])
    }
    return result
}
```

## Layout
tools:showIn
## Guava
Guava는 구글이 작성한 자바 오픈소스 라이브러리
### Guava를 사용하면 좋은 5가지 이유
1. 컬렉션 초기화와 유틸리티
컬렉션 생성을 간단하게 하고 많은 유틸리티 함수가 Maps, Sets등의 컬렉션에 들어가있다.

  ```java
    // java
    final Map<String, Map<String, Integer>> lookup = new HashMap<String, Map<String, Integer>>();

    // guava
    final Map<String, Map<String, Integer>> lookup = Maps.newHashMap();
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

## retrolambda
Android 에서는 retrolambda 플러그인을 사용해서 Lambda를 사용한다
### Lambda를 사용하기 위한 설정
```gradle
// project level
classpath 'me.tatarka:gradle-retrolambda:3.2.2'

// module level
apply plugin: 'me.tatarka.retrolambda'


// java version 표기 1
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
	}
}
// java version 표기 2
retrolambda {
    javaVersion JavaVersion.VERSION_1_7
}
  

```

## IntentService
간단하고 편리하게 Service의 특성을 사용할 수 있다.
클라이언트에서 서비스를 실행하면 Intent를 받아서 Intent Queue 에 순서대로 넣고 IntentService는 Queue에 존재하는 intent를 하나씩 실행하게 된다.
Queue가 모두 비워지면 서비스는 종료한다.
### onHandleIntent
IntentService를 실행하면 IntentService는 onHandleIntent 함수를 호출한다.
### 주의할 점
Queue가 모두 비워지면 IntentService

## onNewintent
Activity 가 foreground 상태에서 Intent에 Extra값을 추가하고
StartActivity를 호출하면 onCreate() 대신에 onNewIntent가 호출되고
그 다음에 noResume()가 호출됨
[onNewIntent알아보기](http://diyall.tistory.com/786)

## dynamic String 적용 - String.xml
string.xml 에 변수 적용하기 
[dynamic String using String.xml?](http://stackoverflow.com/questions/3656371/dynamic-string-using-string-xml)

## 커스텀 font 적용
xmlns:app="http://schemas.android.com/apk/res-auto"

[커스텀 폰트 쉽게 적용하는 방법](http://gun0912.tistory.com/10)

## dp px 변환 계산
[안드로이드 DP 계산기](http://lime.so/6)

## GSON jsonArray 처리
```java
Type listType = new TypeToken<ArrayList<YourClass>>(){}.getType();
List<YourClass> yourClassList = new Gson().fromJson(jsonArray, listType);
```
### 참고
[gson list type](http://stackoverflow.com/a/5554296/6759520)

## Okhttp의 Request body 로그남기기
requestbody는 utf8 형태로 저장하기 때문에 utt8로 인코딩된 데이터를 읽는 로직이 필요하다.
```java
private static String bodyToString(final RequestBody request){
        try {
            final RequestBody copy = request;
            final Buffer buffer = new Buffer();
            copy.writeTo(buffer);
            return buffer.readUtf8();
        } 
        catch (final IOException e) {
            return "did not work";
        }
}
```
### 참고
[stackoverflow answer](http://stackoverflow.com/a/30490779)

## proguard 에러
google 서비스 사용시 에러가 발생하면 proguard 설정에 다음 추가
```gradle
-keep class com.google.android.gms.** { *; }
-dontwarn com.google.android.gms.**
```
### 참고
- [Google Play Services v23 Proguard configuration](http://stackoverflow.com/a/29742809)  

## proguard 에러
com.google.android.gms 에서 duplicate zip entry 에러 발생한다면
```gradle
apply plugin: 'com.google.gms.google-services'
```
위치를 확인, 이 설정은 gradle file의 아래에 위치해야함



## TelephonyManager.listen 사용
통화 상태에 대한 변화 알림을 받는다.
### 등록방법
TelephonyManger의 listen을 호출해 특정 이벤트에 대한 알림을 받을 수 있다.
여기서는 LISTEN_CALL_STATE 이벤트를 사용했다.
```java
TelephonyManager telephony = (TelephonyManager)getSystemService(Context.TELEPHONY_SERVICE);		telephony.listen(phoneStateListener,PhoneStateListener.LISTEN_CALL_STATE);
```
### 리스너
리스너에 대한 예제
```java
private PhoneStateListener phoneStateListener = new PhoneStateListener(){
		@Override
		public void onCallStateChanged(int state, String incomingNumber) {
			super.onCallStateChanged(state, incomingNumber);
			Log.d(TAG, "incomingNumber :" + incomingNumber);
			mIncomingNumber = incomingNumber;

		}
};
```
### 해제방법
이벤트 알림을 받을 필요가 없을 때, 다음과 같이 호출한다.
```java
TelephonyManager telephony = (TelephonyManager)getSystemService(Context.TELEPHONY_SERVICE);
telephony.listen(phoneStateListener,PhoneStateListener.LISTEN_NONE);
```

## Doze 모드
안드로이드 6.0 마쉬멜로우에 새롭게 추가된 절전모드
일정시간동안 움직이지 않는 경우, 디바이스는 Doze 모드에 진입하고 앱들은 배터리 소모가 많은 기능, 네트워크 연결, GPS 스캔 등을 활용할 수 없음..
### 참고
- [Doze와 Foreground Service](https://brunch.co.kr/@huewu/3)

## google-services.json 관련 설명
google-services plugin은 google-services.json 파일에서 서비스 이용에 필요한 프로젝트 정보, 클라이언트 정보를 읽어서 처리한다
### 참고
- [The Google Services Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin)

----



---
## Android와 Annotation
안드로이드에서는 기본적으로 Java 1.6에서 사용되는 Annotation을 지원한다. 대표적으로 @Override가 있다.
### Nullness Annotations
Nullness annotations는 2개의 Annotatinos가 있습니다.
- @NonNull : null을 허용하지 않을 경우
- @Nullable : mull을 허용할 경우  

@NonNull에 null을 추가하게 되면 경고 메시지가 표시된다.
개발에 null을 허용하거나, null을 허용하지 않을 경우에 대하여 미리 Annotations을
적용해두면 추후 개발에 문제가 줄어들 수 있다.



---