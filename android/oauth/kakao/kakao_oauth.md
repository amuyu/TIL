# 개발환경 구성
## repository 설정
```gradle
subprojects {
    repositories {
        mavenCentral()
        maven { url 'http://devrepo.kakao.com:8088/nexus/content/groups/public/' }
    }
}
```
## dependency 추가
```gradle
dependencies {
    // 카카오 로그인 sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'usermgmt', version: project.KAKAO_SDK_VERSION

    // 카카오링크 sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'kakaolink', version: project.KAKAO_SDK_VERSION

    // 카카오톡 sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'kakaotalk', version: project.KAKAO_SDK_VERSION

    // 카카오내비 sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'kakaonavi', version: project.KAKAO_SDK_VERSION

    // 카카오스토리 sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'kakaostory', version: project.KAKAO_SDK_VERSION

    // push sdk를 사용하기 위해 필요.
    implementation group: 'com.kakao.sdk', name: 'push', version: project.KAKAO_SDK_VERSION
}
```

# 앱생성
## 카카오 개발자 센터 등록
내 애플리케이션 > 앱 만들기 를 통해 앱을 생성한다.
Android SDK 를 사용할 때는, 네이티브 앱 키를 사용한다.
## 플랫폼 추가
앱 설정에서 플랫폼 추가를 선택 후, 패키지 명을 입력한다.
## 프로가드 설정
프로가드 설정 시에는 다음 내용을 추가한다.
```
-keep class com.kakao.** { *; }
-keepattributes Signature
-keepclassmembers class * {
  public static <fields>;
  public *;
}
-dontwarn android.support.v4.**,org.slf4j.**,com.google.android.gms.**
```
## 네이티브 앱키 등록
AndroidManifest 에 app key 를 meta-data 로 추가한다.
```xml
<application>
    <meta-data
            android:name="com.kakao.sdk.AppKey"
            android:value="@string/kakao_app_key" />
<application/>
```
## 키 해시 등록
디버그 키 해시와 릴리즈 키 해시를 카카오 애플리케이션 설정에 추가해야 한다.
릴리즈 키 해시는 릴리즈용 키스토어 사용이 필요함
### 디버그 키 해시 구하기
```
keytool -exportcert -alias androiddebugkey -keystore <debug_keystore_path> -storepass android -keypass android | openssl sha1 -binary | openssl base64
```


# 앱내 설정
Application 에서 KakaoSDK.init 을 호출해줘야 한다.
> KakaoSDK를 사용하기 위해선 SDK와 Application을 연결해 주어야 하며, 이때 사용되는 객체는 KakaoAdapter입니다. KakaoAdapter를 통해서 SDK에 필요한 정보를 제공해 주어야 하며 각각 IApplicationConfig와 ISessionConfig를 구현해 주어야 합니다.
https://developers.kakao.com/docs/android/user-management#로그인-사용법
```java
public class GlobalApplication extends Application {
    private static class KakaoSDKAdapter extends KakaoAdapter {
        /**
         * Session Config에 대해서는 default값들이 존재한다.
         * 필요한 상황에서만 override해서 사용하면 됨.
         * @return Session의 설정값.
         */
        @Override
        public ISessionConfig getSessionConfig() {
            return new ISessionConfig() {
                @Override
                public AuthType[] getAuthTypes() {
                    return new AuthType[] {AuthType.KAKAO_LOGIN_ALL};
                }

                @Override
                public boolean isUsingWebviewTimer() {
                    return false;
                }

                @Override
                public boolean isSecureMode() {
                    return false;
                }

                @Override
                public ApprovalType getApprovalType() {
                    return ApprovalType.INDIVIDUAL;
                }

                @Override
                public boolean isSaveFormData() {
                    return true;
                }
            };
        }

        @Override
        public IApplicationConfig getApplicationConfig() {
            return new IApplicationConfig() {
                @Override
                public Context getApplicationContext() {
                    return GlobalApplication.getGlobalApplicationContext();
                }
            };
        }
    }

    @Override
    public void onCreate() {
        super.onCreate();

        KakaoSDK.init(new KakaoSDKAdapter());

        ...
    }
    ...
}
```


# 로그인 버튼
로그인 버튼을 포함하는 FrameLayout
```xml
<com.kakao.usermgmt.LoginButton
    android:id="@+id/com_kakao_login"
    android:layout_width="0dp"
    android:layout_height="wrap_content"
    android:layout_weight="1"
    android:layout_marginBottom="30dp"
    android:layout_marginLeft="20dp"
    android:layout_marginRight="20dp"/>
```

# activity 코드
```java
public class SampleLoginActivity extends Activity {
    private SessionCallback callback;

    /**
     * 로그인 버튼을 클릭 했을시 access token을 요청하도록 설정한다.
     *
     * @param savedInstanceState 기존 session 정보가 저장된 객체
     */
    @Override
    protected void onCreate(final Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.layout_common_kakao_login);

        callback = new SessionCallback();
        Session.getCurrentSession().addCallback(callback);
        Session.getCurrentSession().checkAndImplicitOpen();
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (Session.getCurrentSession().handleActivityResult(requestCode, resultCode, data)) {
            return;
        }

        super.onActivityResult(requestCode, resultCode, data);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Session.getCurrentSession().removeCallback(callback);
    }

    private class SessionCallback implements ISessionCallback {

        @Override
        public void onSessionOpened() {
            redirectSignupActivity();
        }

        @Override
        public void onSessionOpenFailed(KakaoException exception) {
            if(exception != null) {
                Logger.e(exception);
            }
        }
    }

    protected void redirectSignupActivity() {
        final Intent intent = new Intent(this, SampleSignupActivity.class);
        startActivity(intent);
        finish();
    }
}
```

# Client secret 기능 
카카오 로그인에서는 보안 수준을 강화하기 위하여 OAuth 클라이언트 시크릿 기능을 제공합니다.
https://developers.kakao.com/docs/android/user-management#client-secret-기능
```xml
<uses-permission android:name="android.permission.INTERNET" />

<application>
    ...
    <meta-data
        android:name="com.kakao.sdk.ClientSecret"
        android:value="<kakao client secret value>"/>
    ...
</application>
```

# 로그아웃
https://developers.kakao.com/docs/android/user-management#로그아웃
```java
private void onClickLogout() {
    UserManagement.getInstance().requestLogout(new LogoutResponseCallback() {
        @Override
        public void onCompleteLogout() {
            redirectLoginActivity();
        }
    });
}
```

# 앱 연결 해제
https://developers.kakao.com/docs/android/user-management#앱-연결-해제
```java
private void onClickUnlink() {
    final String appendMessage = getString(R.string.com_kakao_confirm_unlink);
        new AlertDialog.Builder(this)
            .setMessage(appendMessage)
            .setPositiveButton(getString(R.string.com_kakao_ok_button),
                new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        UserManagement.getInstance().requestUnlink(new UnLinkResponseCallback() {
                            @Override
                            public void onFailure(ErrorResult errorResult) {
                                Logger.e(errorResult.toString());
                            }

                            @Override
                            public void onSessionClosed(ErrorResult errorResult) {
                                redirectLoginActivity();
                            }

                            @Override
                            public void onNotSignedUp() {
                                redirectSignupActivity();
                            }

                            @Override
                            public void onSuccess(Long userId) {
                                redirectLoginActivity();
                            }
                        });
                        dialog.dismiss();
                    }
                })
            .setNegativeButton(getString(R.string.com_kakao_cancel_button),
                new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                }).show();

}
```

# 사용자 정보 요청
https://developers.kakao.com/docs/android/user-management#사용자-정보-요청
```java
private void requestMe() {
    List<String> keys = new ArrayList<>();
    keys.add("properties.nickname");
    keys.add("properties.profile_image");
    keys.add("kakao_account.email");

    UserManagement.getInstance().me(keys, new MeV2ResponseCallback() {
        @Override
        public void onFailure(ErrorResult errorResult) {
            String message = "failed to get user info. msg=" + errorResult;
            Logger.d(message);
        }

        @Override
        public void onSessionClosed(ErrorResult errorResult) {
            redirectLoginActivity();
        }

        @Override
        public void onSuccess(MeV2Response response) {
            Logger.d("user id : " + response.getId());
            Logger.d("email: " + response.getKakaoAccount().getEmail());
            Logger.d("profile image: " + response.getKakaoAccount()
            .getProfileImagePath());
            redirectMainActivity();
        }

        @Override
        public void onNotSignedUp() {
            showSignup();
        }
    });
}
```

# 사용자 정보 저장
https://developers.kakao.com/docs/android/user-management#사용자-정보-저장
```java
private void requestUpdateProfile() {
    final Map<String, String> properties = new HashMap<String, String>();
    properties.put("nickname", "Leo");
    properties.put("age", "33");

    UserManagement.getInstance().requestUpdateProfile(new UsermgmtResponseCallback<Long>() {
        @Override
        public void onSuccess(Long userId) {
            userProfile.updateUserProfile(properties);
            if (userProfile != null) {
                userProfile.saveUserToCache();
            }
            Logger.d("succeeded to update user profile" + userProfile);
        }

        @Override
        public void onNotSignedUp() {
            redirectSignupActivity(self);
        }

        @Override
        public void onFailure(ErrorResult errorResult) {
            String message = "failed to get user info. msg=" + errorResult;
            Logger.e(message);
        }

        @Override
        public void onSessionClosed(ErrorResult errorResult) {
            redirectLoginActivity(self);
        }

    }, properties);
}
```

# 토큰 정보 얻기
https://developers.kakao.com/docs/android/user-management#토큰-정보-얻기
```java
private void requestAccessTokenInfo() {
    AuthService.getInstance().requestAccessTokenInfo(new ApiResponseCallback<AccessTokenInfoResponse>() {
        @Override
        public void onSessionClosed(ErrorResult errorResult) {
            redirectLoginActivity(self);
        }

        @Override
        public void onNotSignedUp() {
            // not happened
        }

        @Override
        public void onFailure(ErrorResult errorResult) {
            Logger.e("failed to get access token info. msg=" + errorResult);
        }

        @Override
        public void onSuccess(AccessTokenInfoResponse accessTokenInfoResponse) {
            long userId = accessTokenInfoResponse.getUserId();
            Logger.d("this access token is for userId=" + userId);

            long expiresInMilis = accessTokenInfoResponse.getExpiresInMillis();
            Logger.d("this access token expires after " + expiresInMilis + " milliseconds.");
        }
    });
}
```