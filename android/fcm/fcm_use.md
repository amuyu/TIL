## FCM 사용
### 개요
FCM(Firebase Cloud Messaging)은 기본 GCM 구조에 새로운 기능이 추가된 새로운 version 이다
GCM 사이트를 방문하면 FCM을 사용할 것을 추천하고 있다. [FCM 사이트](https://firebase.google.com/docs/cloud-messaging/)

### Firebase 프로젝트 추가
Firebase를 사용하기 위해서는 Firebase 프로젝트 생성이 필요하다.
1. [Firebase console](https://console.firebase.google.com/) 사이트로 이동한다.
2. **Create New Project** 를 선택해 프로젝트를 생성한다.
3. 프로젝트 생성 후, **Add Firebase to your Android App** 을 선택하고 패키지명을 입력한다.
3. 구성 파일`google-services.json`이 다운로드되면 Android 스튜디오에서 프로젝트에 추가한다.

### SDK 추가
Firebase 라이브러리 사용을 위해 몇 가지 설정을 추가한다.
#### 프로젝트 build.gradle 설정
firebase를 이용하기 위해서는 google-service를  **3.0.0** 이상 버전을 사용해야 한다.
```gradle
buildscript {
    // ...
    dependencies {
        // ...
        classpath 'com.google.gms:google-services:3.0.0'
    }
}
```

#### 앱 build.gradle 설정
google-service 플러그인과 firebase 설정을 추가한다.
```gradle
apply plugin: 'com.android.application'

android {
  // ...
}

dependencies {
  // ...
  compile 'com.google.firebase:firebase-core:9.4.0'
}

// ADD THIS AT THE BOTTOM
apply plugin: 'com.google.gms.google-services'
```

### Android 수정
기본 설정은 완료되었고 토큰 발급, 메시지 수신을 위한 소스를 추가한다.
#### Androidmanifest 설정
GCM 설정보다 훨씬 설정이 간편해졌다.InstanceID 생성을 위한 서비스와 메시지를 수신할 서비스만 있으면 된다.
```xml
<!-- [START firebase_service] -->
<service
    android:name=".MyFirebaseMessagingService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
<!-- [END firebase_service] -->
<!-- [START firebase_iid_service] -->
<service>
    android:name=".MyFirebaseInstanceIDService">
    <intent-filter>
        <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter>
</service>
<!-- [END firebase_iid_service] -->

```

#### InstanceIDListenerService Java
새로운 토큰이 업데이트 될 때, Callback되는 서비스
```java
public class MyFirebaseInstanceIDService extends FirebaseInstanceIdService {
  /**
   * 새로 토큰이 업데이트 되었을 때 여기로 Callback이 옵니다.
   */
  @Override
  public void onTokenRefresh() {
      String token = FirebaseInstanceId.getInstance().getToken();
      // 이제 이 이후에 Token을 등록하는 코드를 넣어주세요.
      ...
  }
}
```

#### GcmListenerService Java
메시지를 수신하는 서비스, Foreground일 때, 호출된다.
```java
public class MyFirebaseMessagingService extends FirebaseMessagingService {
  /**
   * Foreground에서 메세지를 수신했을 때의 Callback입니다.
   * @param message 레퍼런스(https://developers.google.com/android/reference/com/google/firebase/messaging/RemoteMessage)를 참조하세요.
   */
  @Override
  public void onMessageReceived(RemoteMessage message){
    String from = message.getFrom();
    Map data = message.getData();
    // 이제 이 이후에 데이터로 알림 등을 만들어주는 코드를 넣어주세요.
    ...
  }
}
```

#### 백그라운드 푸쉬
백그라운드에 있을 때는 그냥 알림이 뜨게 되며, 알림을 누르면 앱의 시작 지점 Activity로 Intent를 호출한다.
##### FCM 백그라운드 push 처리 하는 방법
FCM 메시지는 두가지 타입이 있음.
- display-messages: These messages only work when your app is in foreground
- data-messages: These messages work even if your app is in background

`data-messages` 형태로 보내기 위해서는 message body에 `notification`값을 포함하지 않고 `data`를 포함해서 전송하면 data-messages 로 전송할 수 있다. 
- 참고
 - [How to handle notification when app in background in firebase](http://stackoverflow.com/questions/37711082/how-to-handle-notification-when-app-in-background-in-firebase)

#### 토큰 가져오기
토큰을 가져오는 함수 호출
```java
String token = FirebaseInstanceId.getInstance().getToken();
```

#### subscribe Topic 추가
Topic subscribe 하는 부분 추가
```java
FirebaseMessaging.getInstance().subscribeToTopic("news");
```

### Server key 확인
1. `Firebase Console` 로 이동한다.
2. 프로젝트를 선택한다.
3. `프로젝트 설정`으로 이동한다.
4. `클라우드 메시징` 탭 으로 이동한다.
5. 프로젝트 키의 `서버 키`를 확인한다.

## 참고
- [Android에서 FCM으로 덜 삽질하며 마이그레이션 하기](http://dev.frientrip.com/android/2016/06/03/android-gcm-to-fcm.html)
- [Set Up a Firebase Cloud Messag...Android](https://firebase.google.com/docs/cloud-messaging/android/client)
- [Add Firebase to your app](https://firebase.google.com/docs/server/setup)
- [About Firebase Cloud Messaging Server](https://firebase.google.com/docs/cloud-messaging/server)