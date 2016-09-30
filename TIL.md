# TIL
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


## Dipendency Injection
요즘 안드로이드 개발에서 Dependency Injection 방법을 이용한 개발 방법이 이슈이다.
### 안드로이드에서 @Inject와 @Test 사용의 좋은 점
#### DI의 이점
DI의 이점은 크게 세 가지를 들 수 있다.
- 객체의 생성 주기를 제어한다.
  DI 스타일이 유행하기 전까지는 한 번 생성해서 계속 재활용할 객체의 sigleton 패턴을 직접 구현했다.
  객체 생성 지점을 통제하기 위해서, getInstance() 메서드를 추가하는 등 각 클래스마다 코드를 추가해야 했다.
- 객체에 부가 기능을 추가한다.
  AOP(Aspect Oriented Programming)을 이용하면, 의존하는 객체에 같은 규약을 유지한 채로 로깅, 보안, 트랜잭션 처리, Exception 처리 등의 부가 기능을 추가할 수 있어 반복되는 코드를 줄이고 유연하게 각종 운영 정책을 변경할 수 있다.
- 실행 환경에 따라 구현 객체를 바꿔치기한다.  


#### Andorid에서도 @Inject가 의미 있을까
- 용량에 부담이 된다.
  모바일 기기의 한정된 자원 때문
- 성능에 부담이 된다.
  프레임워크를 사용하면 실행 시에 콜스택이 늘어날 수 있다. 의존 관계를 해석하기 위해 앱 초기 로딩에 더 많은 일을 해야 할 가능성도 크다.
- Dalvik에서는 cglib과 같은 런타임 바이트 코드 생성 라이브러리를 사용할 수 없다.
- 앱 규모가 작아서 DI의 유연성으로 얻는 이득이 크지 않다.
- 안드로이드 기본 프레임워크와 DI 프레임워크의 조화를 고려해야 한다.



### Dagger2
Square에서 만든 Dagger를 Google이 Fork하여 개선한 DI(Dependency injection) 프레임워크
의존성에 따른 객체생성을 추상화하여 보일러플레이트 코드를 줄이고, 변화에 유연하게 만든다.
Dagger2를 이용해 여러 클래스에 의존하는 MVP 모델의 Presenter와 Model객체를 깔끔하게 구현할 수 있다.
#### 중요 Annotation
- @Module : Module로 구성되고, 내부에서 주입할 객체들을 provide해주는 것
- @Provides : 객체 주입에 필요한 내용을 리턴해주는 것
- @Component : Module과 Inject간의 Bridge 같은 역할
- @Inject : 객체의 주입.

#### 사용 방법 정리
##### gradle 설정 추가
```gradle
compile 'com.google.dagger:dagger:2.0'
```
##### 모델 정의
Motor.java
```java
public class Motor { 
    private int speed;
 
    public Motor() {
        this.speed = 0;
    }
 
    public int getSpeed() {
        return speed;
    }
 
    public void accelerate(int value) {
        speed = speed + value;
    }
 
    public void setSpeed(int value) {
        speed = value;
    }
 
    public void brake() {
        speed = 0;
    }
}

```
Bike.java
```java
public class Bike {
 
    private Motor motor;
 
    public Bike(@NonNull Motor motor) {
        this.motor = motor;
    }
 
    public void increaseSpped() {
        this.motor.setSpeed(this.motor.getSpeed() + 1);
    }
 
    public void decreaseSpeed() {
        if (this.motor.getSpeed() > 0) {
            this.motor.setSpeed(this.motor.getSpeed() - 1);
        }
    }
 
    public int getSpeed() {
        return this.motor.getSpeed();
    }
}
```
##### Module 정의
MotorModule.java
```java
@Module
public class MotorModule {
 
    @Provides
    Motor provideMotor() {
        return new Motor();
    }
}
```
BikeModule.java
```java
@Module(
        includes = MotorModule.class
)
public class BikeModule {
 
    @Provides
    Bike provideBike(Motor motor) {
        return new Bike(motor);
    }
}
```
#### Component 정의
MainActivityComponent.java
```java
@Component(
        ...
        modules = {
                ...
                BikeModule.class
        }
)
public interface MainActivityComponent extends ActivityComponent {
    void inject(@NonNull MainActivity mainActivity);
}
```
이렇게 Component를 정의하고 컴파일 하면 "DaggerMainActivityComponent"가 만들어 진다. 이걸 가지고 실질적으로 객체 주입을 진행할 수 있다.
```java
aggerMainActivityComponent.builder()
        ...
        .bikeModule(new BikeModule())
        .build();
```
### Inject 정의
```java
@Inject
Bike bike;
```

#### 참고
- [Android깨알팁4-Dagger2](https://medium.com/@jsuch2362/android-%EA%B9%A8%EC%95%8C%ED%8C%81-4-dagger2-7f38cd9cb11b#.owet5o6uu)
- [Android 개발에서 Dagger2 이용해보기](http://drcarter.tistory.com/169)

### 참고
- [Android에서 @Inject, @Test](http://d2.naver.com/helloworld/342818)

---
