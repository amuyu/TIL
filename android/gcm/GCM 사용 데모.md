## GCM 사용 데모

### 앱 등록하기
안드로이드 GCM 서비스를 사용하기 위해 앱을 등록해야 한다 
앱 등록을 위해 GCM 사이트로 이동한다. [GCM사이트](https://developers.google.com/cloud-messaging/)
1. 상단에 TRY IT ON ANDROID 버튼 클릭
2. 이동한 페이지 하단에 `add Cloud Messaging to your existing app` 을 클릭
3. 페이지 중간에 `Get a configuration file` 선택
4. GCM을 사용하기 위한 앱을 등록한다 (앱이름, 패키지명)
5. `Continue Choose an configure services` 클릭
6. 등록 결과 화면의 `Cloud Messaging` 탭을 보면 `ENABLE GOOGLE COLUD MESSAGING` 버튼 클릭해서 활성화

### GCM 설정 파일 만들기
GCM 서비스가 configuration file로 처리할 수 있게 업그레이드 되었다.
1. 등록 결과 화면에서 `Generate configuration files` 선택한다.
2. `Download google-services.josn` 파일을 다운로드 한다.
3. 다운로드 한 설정파일을 Android 프로젝트의 app 디렉토리 안으로 복사한다.

### build.gradle 설정
라이브러리 설치와 클래스패스 설정
1. 프로젝트 build.gradle을 열어서 dependencies 추가 `classpath 'com.google.gms:google-services`
2. app 설정하는 build.gradle을 열어서 dependencies 추가 `compile "com.google.android.gms:play-services`

### AndroidManifest.xml 설정
GCM 서비스를 만들기 위한 메타정보 정의
- permission
  ```xml
  	<!-- [START gcm_permission] -->
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <!-- [END gcm_permission] -->
  ```
- receiver
  ```xml
    <!-- [START gcm_receiver] -->
    <receiver
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="net.saltfactory.demo.gcm" />
        </intent-filter>
    </receiver>
    <!-- [END gcm_receiver] -->
  ```
- Listener Service
  ```xml
  	<!-- [START gcm_listener_service] -->
    <service
        android:name="net.saltfactory.demo.gcm.MyGcmListenerService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        </intent-filter>
    </service>
    <!-- [END gcm_listener_service] -->
  ```
- InstanceID Listener Service
  ```xml
  	<!-- [START instanceId_listener_service] -->
    <service
        android:name="net.saltfactory.demo.gcm.MyInstanceIDListenerService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.android.gms.iid.InstanceID" />
        </intent-filter>
    </service>
    <!-- [END instanceId_listener_service] -->
  ```
- Registration Service
  ```xml
  	<!-- [START gcm_registration_service] -->
    <service
        android:name="net.saltfactory.demo.gcm.RegistrationIntentService"
        android:exported="false"></service>
    <!-- [END gcm_registration_service] -->
  ```

### 안드로이드 만들기
Demo를 진행할 화면과 서비스를 추가한다
#### MainActivity
Demo의 메인 화면
##### UI
인스턴스 아이디를 가져올 수 있게 버튼과 아이디를 출력할 TextView를 추가한다
##### 서비스 연동
- LocalBroadcastManager에 GCM 등록 결과를 받을 브로드캐스드를 등록/해제
- GCM 을 등록한다

#### RegistrationIntentService
Instance ID를 가지고 토큰을 가져오는 작업을 한다
```java
InstanceID instanceID = InstanceID.getInstance(this);
String token = null;
try {
    synchronized (TAG) {
        // GCM 앱을 등록하고 획득한 설정파일인 google-services.json을 기반으로 SenderID를 자동으로 가져온다.
        String default_senderId = getString(R.string.gcm_defaultSenderId);
        // GCM 기본 scope는 "GCM"이다.
        String scope = GoogleCloudMessaging.INSTANCE_ID_SCOPE;
        // Instance ID에 해당하는 토큰을 생성하여 가져온다.
        token = instanceID.getToken(default_senderId, scope, null);

        Log.i(TAG, "GCM Registration Token: " + token);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

#### MyInstanceIDListenerService
Instace ID를 획득하기 위한 리스너를 상속받아서 토큰을 갱신하는 코드를 추가한다
```java
public class MyInstanceIDListenerService extends InstanceIDListenerService {
    private static final String TAG = "MyInstanceIDLS";
    @Override
    public void onTokenRefresh() {
        Log.d(TAG, "onTokenRefresh");
        Intent intent = new Intent(this, RegistrationIntentService.class);
        startService(intent);
    }
}
```
#### MyGcmListenerService
GCM으로부터 메시지가 도착하면 어떻게 사용할지에 대한 내용을 정의한다
```java
public class MyGcmListenerService extends GcmListenerService {

    private static final String TAG = "MyGcmListenerService";

    /**
     *
     * @param from SenderID 값을 받아온다.
     * @param data Set형태로 GCM으로 받은 데이터 payload이다.
     */
    @Override
    public void onMessageReceived(String from, Bundle data) {
    	// title, message
        String title = data.getString("title");
        String message = data.getString("message");
    }
}    
```

#### 앱 실행
Andoird 설정은 모두 끝났고 빌드해서 앱을 실행하여 토큰을 획득하면 화면에 Token을 출력할 것이다

### Node.js 를 이용한 GCM Provider 만들기
아래 소스코드 링크에서 node 프로젝트를 다운 받는다. node 폴더로 이동 후,
`node-gcm`이 필요하기 때문에 설치한다.
```bash
npm install node-gcm
```
`gcm-provider.js` 파일에 `server_api_key`와 `token`을 수정한다.
```js
var server_api_key = ‘GCM 앱을 등록할때 획득한 Server API Key’;
var token = ‘Android 디바이스에서 Instance ID의 token’;
```
수정 후, 메시지를 발송해보자.
```bash
node gcm_provider.js
```
정상적으로 발송되면 아래와 같은 결과가 나온다.
```json
{ multicast_id: 5338188998685927000,
  success: 1,
  failure: 0,
  canonical_ids: 0,
  results: [ { message_id: '0:1470199426943783%652f5c41002efde3' } ] }
```

---
### InstanceID 관련 참고 사항
Instance ID is stable but may become invalid, if:
- App deletes Instance ID
- Device is factory reset
- User uninstalls the app
- User clears app data
[InstanceID 구글 페이지](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID)


### GCM 메시지 종류
GCM이 제공하는 메시지의 종류는 크게 4가지가 있습니다.
- 다운스트림 메시지
  이것이 우리가 일반적으로 말하는 푸시 메시지입니다.
- 업스트림 메시지
  클라이언트 앱에서 서버로 메시지를 발송하는 메시지입니다. (XMPP를 사용)
- 디바이스 그룹 메시지
  디바이스들은 특정 그룹에 가입하고 서버는 해당 그룹의 키로 메시지를 발송하면 해당 그룹에 가입되어 잇는 모든 디바이스들이 그 메시지를 수신받음
- 토픽 메시지
  Publish - Subscribe 패턴을 가지는 방식으로 디바이스가 특정 주제를 구독하며 해당 주제에 푸시를 발송하면 구독하는 모든 디바이스가 메시지를 수신함 (공지사항)
  message(payloads) 크기는 2KB



### 소스코드
https://github.com/saltfactory/saltfactory-android-tutorial/tree/gcm-demo

### 참고
[최신 Android Studio, Google Cloud Messaging 3.0을 이용하여 Android 푸쉬 서비스 구현하기](http://blog.saltfactory.net/android/implement-push-service-via-gcm.html)
[GCM Topic을 이용해서 한번의 발송으로 단체 푸시를 발송하기](http://theeye.pe.kr/archives/2648)
[The Google Services Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin)


