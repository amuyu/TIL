
# 지문인증
Android Keystore 시스템과 함께 사용!
Android 지문 아이콘을 사용해야 한다 (c_fp_40px.png)
## 환경
### sdk
샘플 프로젝트 기준
compile/target sdk 24
minsdk 23
### 권한
```xml
<uses-permission
        android:name="android.permission.USE_FINGERPRINT" />
```
## 가이드
1. Andorid SDK 도구 수정 버전 24.3 설치
2. Settings > Security > Fingerprint 지문 등록
3. 터치 이벤트 에뮬레이트
```bash
adb -e emu finger touch <finger_id>
```

## 신한카드
UI 상으로는 등록하는 것처럼 보이지만 실제로는 설정의 정보를 사용
- 지문정보 사전등록 필요 (시스템>설정을 통해 지문을 등록해야 사용 가능)
- 지문인증 이용 동의 (권한)
- 사용자 인증
- 지문 스캔화면 표시 (지문인증)
- 지문 등록

## Fingerprint api 사용
- 설정에 등록된 정보를 기준으로 확인

## 지문 인증
지문 스캔을 통해 사용자를 인증하려면 FingerprintManager.authenticate() 호출

## 자격 증명 확인 (참고)
사용자가 최근에 기기를 마지막으로 잠금 해제한 방식에 따라 앱에서 사용자를 인증함


# Android KeyStore 시스템
암호화 키를 컨테이너에 저장해야 하므로 이 키를 기기에서 추출해내기가 어렵다
키 사용 시기와 사용 방법을 제한하는 기능이 있다.
- 주요 머터리얼은 애플리케이션 프로세스에 들어가지 않는다. 시스템 프로세스로 들어간다.
- 안드로이드 기기의 보안 하드웨어에 바인딩 될 수 있다.???

## 환경
- sdk 18이상?
- setInvalidatedByBiometricEnrollment 24이상

## 키 사용 승인
키의 불법 사용을 줄이기 위해, 키를 생성하거나 가져올 대, 적법한 키 사용을 앱에서 지정하도록 한다.
키를 생성하거나 가져온 후에는 승인을 변경할 수 없다. 키가 사용될 때마다 Android KeyStore에 승인이 시행된다.
- 암호화 : 키를 사용할 수 있는 승인된 키 알고리즘
- 시간적 유효성 간격 : 키 사용이 승인되는 시간 간격
- 사용자 인증 : 사용자가 인증된 경우에만 키를 사용할 수 있음

## Keychain, Keysotre
keychain : 여러 앱이 자격 증명을 사용하는 경우
keystore : 단일 앱에서 사용하는 경우, Android KeyStore 제공자 사용

## Android KeyStore 제공자 사용
키를 생성하거나 AndroidKeyStore로 가져올 경우, 사용자가 인증된 후에만 키 사용을 승인하도록 지정할 수 있습니다.

# 참고
[지문인증](http://developer.android.com/intl/ko/about/versions/marshmallow/android-6.0.html#fingerprint-authentication)
[지문인증라이브러리](https://github.com/ajalt/reprint)
[KeyStore 시스템](https://developer.android.com/training/articles/keystore.html?hl=ko)
