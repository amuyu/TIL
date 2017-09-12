# Firebase 예제로 구글/페이스북 OAuth 사용

Firebase에서 제공하는 [인증](https://firebase.google.com/products/auth/)기능을 사용해보자
Firebase에서는 이메일/비밀번호 계정, 전화 인증, Google, Twitter, Facebook, GitHub 로그인 등을 지원해서
이를 사용한 인증 시스템을 손쉽게 구축하도록 도와준다. 이 중에서 많이 사용하는 Google과 Facebook 로그인을 직접 사용해보자

## 예제 다운로드
먼저 , 다음의 [Github 저장소](https://github.com/firebase/quickstart-android) 에서 Firebase 에서 제공하는
quickstart-android를 다운로드 받아 안드로이드 스튜디오에서 연다. 프로젝트 안에는 Firebase에서 제공하는 여러 기능들의 샘플이 있는데
이 중에서, `auth-app` 이 인증에 대한 샘플 앱이다.

## 구글 인증
[Firebase 콘솔] 로 이동해서 테스트 하기 위한 프로젝트를 추가한다.
![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase/1.png?raw=true)

앱 추가  화면

프로젝트 관리 화면으로 이동하면, 사이드 메뉴에서 `Authentication` 을 선택 > Authentication 화면에서 `로그인 방법` 선택 >
로그인 제공업체 중 Google 선택하면 아래와 같은 화면이 나오는데 사용 설정으로 변경하고 저장한다.





# 참고
[Android에서 Facebook 로그인을 사용하여 인증하기](https://firebase.google.com/docs/auth/android/facebook-login)
[Firebase Auth 와 함께 구글/페이스북 로그인](https://medium.com/askdjango-android/firebase-auth-와-함께-구글-페이스북-로그인-6ce65c124e5)
[Firebase Authentication 이용하기](https://isjang98.github.io/blog/Firebase-Authentication)
