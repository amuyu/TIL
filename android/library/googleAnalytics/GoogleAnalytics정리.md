# GoogleAnalytics
웹사이트 또는 모바일 앱에서 기본 데이터 수집 용도로 사용
## 애널리닉스 시작하기
1. Google 애널리틱스 계정을 생성 
  [google.com/analytics](http://www.google.co.kr/analytics/) 로 이동하여 오른쪽 상단의 로그인 또는 계정 만들기 클릭하고 안내를 따른다.
2. 만든 계정에서 앱을 추적하기 위한 속성을 설정(추가)
  1) 애널리틱스 계정에서 관리 탭을 선택
  2) 계정 항목에서 계정을 선택하고 속성항목의 드롭다운 메뉴에서 새 속성 만들기를 선택
  3) 웹사이트 또는 모바일 앱을 선택
  4) 웹사이트 또는 앱 이름을 입력
  5) (웹에만 해당) 웹사이트 URL 입력
  6) 업종 카테고리 선택
  7) 보고서 시간대 선택
  8) 추적 ID 가져오기 선택
3. 안내에 따라 웹 또는 모바일 앱 추적을 설정

## Firebase 애널리틱스와 Google 애널리틱스 비교
Firebaes는 ios및 Android 앱을 만들 때 GOogle 서비스를 간편하게 이용할 수 있는 모바일 개발 플랫폼입니다.
Firebase 애널리틱스는 Firebase SDK에 번들로 제공되므로 Google 애널리틱스를 사용하려는 경우에도 
항상 Firebase 애널리틱스를 사용하는 것이 좋습니다.
- 앱만 있는 기업 : Firebase 애널리틱스 사용
- 웹사이트만 있는 기업 : Google 애널리틱스 사용
- 앱과 웹사이트가 모두 있는 기업 : Firebase 애널리틱스와 Google 애널리틱스 모두 사용  

Firebase를 Google 애널리틱스에 연결하면 Google 애널리틱스 사용자 인터페이스에서 바로 Firebase 애널리틱스 보고서를
볼 수 있습니다.
Firebase 애널리틱스|Google 애널리틱스 360
---|---
앱용으로 맞춤 설계된 이벤트 기반의 데이터 수집 모델|화면 조회/페이지뷰 데이터 수집 모델
무료로 무제한 이벤트 보고 가능|	웹사이트나 앱에서 애널리틱스 360에 전송하는 모든 데이터에 애널리틱스 360 조회수 한도 및 가격 정책이 적용됨
Google의 모바일 개발자 플랫폼인 Firebase의 통합 기능|독립형 애널리틱스 제품, Google 애널리틱스 360 도구 모음의 일부
'처음 열 때', 인앱 구매 및 기타 주요 이벤트 자동 측정|개발자가 명시적으로 화면 조회 추적을 시작하고 수동으로 앱 이벤트를 측정해야 함
여러 앱의 롤업 없음|	롤업 속성(웹 속성과 모바일 앱 속성의 롤업 포함)
애널리틱스 360 SLA가 적용되지 않음|	애널리틱스 360 SLA가 적용됨


### 참고
[안드로이드 구글 애널리틱스](http://ggari.tistory.com/345)
[Try Analytics for Android](https://developers.google.com/analytics/devguides/collection/android/v4/start?configured=true)
[Add Analytics to Your Andoird App](https://developers.google.com/analytics/devguides/collection/android/v4/?configured=true)
[모바일 앱에 대한 새 속성 설정](https://support.google.com/analytics/answer/2614741?hl=ko)
