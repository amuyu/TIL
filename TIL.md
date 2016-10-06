# TIL
# 코딩
## Builder Pattern
객체 생성 시, Builder 패턴을 사용하면 파라미터가 많을 경우 제공 상태를 일관성 있게 하고, object를 생성시킬 때, step-by-step으로 만들 수 있도록 할 수 있다.
### 작업순서
1) class 안에 중첩 static class를 생성시키고, 바깥쪽 class의 argument들을 안쪽 static class(builder class)로 옮긴다. 바깥쪽 class의 생성자는 private로 선언해서 직접적인 생성을 막는다.
2) builder class의 생성자를 public static으로 선언하고 필요한 파라메터들을 요청한다.
3) builder class에는 선택적 파라메터에 대한 setter method가 있어야한다. 그리고 선택적 인자를 설정한 후에도 같은 builder object를 리턴해야한다.
4) 마지막으로로 클라이언트 프로그램이 요청하는 object를 받을 수 있도록 build method를 만드는데, build method에서는 바깥쪽 class의 생성자가 builder 클래스의 인자를 받을 수 있도록 제공한다.


# 안드로이드
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
