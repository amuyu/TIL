# Firebase 예제로 구글/페이스북 OAuth 사용

Firebase에서 제공하는 [인증](https://firebase.google.com/products/auth/)기능을 사용해보자
Firebase에서는 이메일/비밀번호 계정, 전화 인증, Google, Twitter, Facebook, GitHub 로그인 등을 지원해서
이를 사용한 인증 시스템을 손쉽게 구축하도록 도와준다. 이 중에서 많이 사용하는 Google과 Facebook 로그인을 직접 사용해보자

## 예제 다운로드
먼저 , 다음의 [Github 저장소](https://github.com/firebase/quickstart-android) 에서 Firebase 에서 제공하는
quickstart-android를 다운로드 받아 안드로이드 스튜디오에서 연다. 프로젝트 안에는 Firebase에서 제공하는 여러 기능들의 샘플이 있는데
이 중에서, `auth-app` 이 인증에 대한 샘플 앱을 사용합니다.

## 구글 인증
### 프로젝트 추가
[Firebase 콘솔](https://console.firebase.google.com) 로 이동해서 테스트 하기 위한 프로젝트를 추가한다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/1.png?raw=true)

### 앱 추가
프로젝트 개요 화면에서 `Android 앱에 Firebase 추가`를 선택합니다.

![앱 추가](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/3.png?raw=true)

앱 정보를 입력합니다. 패키지 이름과 서명 인증서를 입력합니다. 선택 사항으로 적혀있지만
인증 기능을 사용하기 위해서 `디버그 서명 인증서`를 입력해야 합니다. 이 곳 입력에 필요한 SHA-1 지문을 얻는 방법을 알아보려면
[클라이언트 인증](https://developers.google.com/android/guides/client-auth?hl=ko) 을 참조하세요.
다 입력했으면 앱 등록을 선택합니다. 그러면 구성 파일을 다운로드 할 수 있는데
다운 받아 안내하고 있는 위치에 구성 파일을 이동합니다.

![구성파일](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/4.png?raw=true)

다음 단계인 플러그인 설정은 샘플앱에 이미 되어 있으므로 패스합니다. 다음으로 구글 인증을 사용하도록 설정해보겠습니다.

### 인증 방법 설정
프로젝트 관리 화면의 사이드 메뉴에서 `Authentication` 을 선택 > Authentication 화면에서 `로그인 방법` 선택 >
로그인 제공업체 중 Google 선택하면 아래와 같은 화면이 나오는데 사용 설정으로 변경하고 저장한다.  

![로그인방법](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/2.png?raw=true)

### 앱 실행
이제 구글 인증을 위한 설정은 끝났다. auth-app이 빌드되도록 셋팅하고 실행한다

![AndroidStudio](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/6.png?raw=true)

GoogleSignInActivity를 선택하고 로그인 버튼을 선택한다.
다음 화면에서 구글 계정을 선택하고 다음으로 진행하면 인증에 성공한 것을 볼 수 있다.
![app](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/15.png?raw=true)

인증한 정보는 Firebase 관리 화면에서도 볼 수 있는데 프로젝트 관리 화면으로 이동해서 `Authentication`
으로 이동하면 아래와 같이 인증한 계정 정보를 볼 수 있다.

![Authentication](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/7.png?raw=true)

## 페이스북 인증
### 프로젝트 추가
구글 인증에서 사용한 프로젝트를 그대로 사용한다.

### 앱 추가
구글 인증에서 사용합 앱을 그대로 사용한다.

### 페이스북 앱 추가
[페이스북 개발자](https://developers.facebook.com) 사이트로 이동해서 앱을 추가한다.
앱 추가를 선택해서 앱 이름과 연락처 이메일을 입력하고 앱 ID를 만든다

![facebook](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/8.png?raw=true)

앱 ID가 만들어지면 제품 선택화면으로 이동하는데 `Facebook 로그인`을 선택한다. 그리고 Android를 선택한다.
그러면 SDK 다운로드 화면으로 이동하는데 우리는 Firebase-auth 를 사용할 것이므로 다음으로 넘어간다
`Android 프로젝트에 대해 Facebook에 알리기` 단계에서 패키지 이름과 기본 액티비티 클래스 이름을 설정한다.
샘플앱의 패키지명과 클래스 이름을 입력한다.

![package](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/10.png?raw=true)

다음으로 해시 추가를 해야한다 안내되어 있는데로 명령을 복사해서 키를 생성한 후, 붙여넣는다.
SSO 옵션을 활성화라고 하는데 활성화하고 다음으로 넘어간다.
다음은 매니페스트 수정이다. 안내되어 있는대로 설정할 내용들을 각각의 위치에 복사해서 붙여넣는다.
그 이후부터는 그냥 다음으로 넘어간다.
페이스북에서 앱 추가 설정은 완료되었다.

### 인증 방법 설정
프로젝트 관리 화면의 사이드 메뉴에서 `Authentication` 을 선택 > Authentication 화면에서 `로그인 방법` 선택 >
로그인 제공업체 중 Facebook을 선택한다. 그러면 아래와 같이 ID와 비밀번호를 입력하라고 나오는데
페이스북 앱 설정 화면 사이드메뉴의 대시보드에서 인증에 필요한 정보인 앱 ID와 앱 비밀번호를 확인할 수 있다.
확인한 내용을 입력한다. 그리고 비밀번호 아랫부분에 URI가 있는데 이 URI를 복사하고 저장한다.

![Authentication](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/11.png?raw=true)

복사한 URI 를 페이스북 앱 설정 화면에서 사이드 메뉴의 `Facebook 로그인`을 선택하면
유효한 OAuth 리디렉션 URI 입력 필드에 붙여넣기 한다.

![Authentication](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/12.png?raw=true)

인증 방법 설정이 완료되었다.

### 앱 실행
앱 설정을 변경했으므로 다시 auth-app이 빌드하고 실행한다
FacebookLoginActivity를 선택하고 `Facebook으로 계속` 버튼을 선택한다. 다음 화면에서
페이스북 계정을 선택하고 다음으로 진행하면 인증에 성공한 것을 볼 수 있다.

![app](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/16.png?raw=true)

인증한 정보는 Firebase 관리 화면에서도 볼 수 있는데 프로젝트 관리 화면으로 이동해서 `Authentication`
으로 이동하면 아래와 같이 인증한 계정 정보를 볼 수 있다.

![Authentication](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/13.png?raw=true)


# 참고
[Android 구글 로그인을 사용하여 인증하기](https://firebase.google.com/docs/auth/android/google-signin?hl=ko)
[Android에서 Facebook 로그인을 사용하여 인증하기](https://firebase.google.com/docs/auth/android/facebook-login)
[Firebase Auth 와 함께 구글/페이스북 로그인](https://medium.com/askdjango-android/firebase-auth-와-함께-구글-페이스북-로그인-6ce65c124e5)
[Firebase Authentication 이용하기](https://isjang98.github.io/blog/Firebase-Authentication)
