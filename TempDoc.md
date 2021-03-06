# TIL
# etc
## drone
Open Source UAV Platform

# Mac
## Homebrew
Apple에서 제공하지 않는 유용한 패키지 관리자 설치
## OpenCV 설치
```bash
brew tap homebrew/science
brew install opencv3 --HEAD --with-contrib
echo /usr/local/Cellar/opencv3/HEAD-85e01f3_4/lib/python2.7/site-packages >> /usr/local/lib/python2.7/site-packages/opencv3.pth
```
[Installing opencv-3.1.0 on MacOS Sierra to use with python](http://stackoverflow.com/a/39775148/6811452)

# 프로토콜?
## http websocket
http w3c 문서...가 재밋다...?
http 는 rfc 문서..? 고통의 2616 문서
https://tools.ietf.org/html/rfc2616

# 업무
## Expert Beginner
[더 이상 배우려 하지 않는 개발자](https://medium.com/@jwyeom63/더-이상-배우려-하지-않는-개발자-expert-beginner의-등장-dd40c40aeedf)

## 개발 프로세스
[토스랩, 이렇게 일하고 있습니다.](http://tosslab.github.io/process/2016/02/25/how-to-work-in-tosslab.html)

## 코드리뷰
### Airbnb에서 사용하는 코드리뷰 툴
https://www.phacility.com - 근데 유료
### 요청 방법(토스랩)
- Title
  - Featur/Bug-fix 건
  - 어떤 목적
- 어떤 이슈와 연결됐는지
- Description
  - 어떤 로직을 추가/수정했는지
  - 어떻게 추가 수정했는지  

###  코드리뷰 Tool
Upsource
[Code review process with JetBrains UpSource ](http://www.slideshare.net/oprohonnyi/code-review-process-with-jetbrains-upsource)
GitHub 의 PullRequest 에 대한 개념을 이해해야 할듯


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
## volatile
멀티쓰레딩 환경 시 동기화
## json to pojo
http://www.jsonschema2pojo.org/
## Gvim
### CommandT 플러그인
[Vim에서 파일브라우징을 위한 플러그인](https://blog.outsider.ne.kr/608)
[Getting Command-T Working on Windows](http://chrislaco.com/blog/gettimg-command-t-working-on-windows/)
### UTF-8 한글
```
set tabstop=4
set encoding=cp949
set fileencodings=utf-8,cp949
set langmenu=cp949
set guifont=Gulimche:h12:cHANGEUL
set lines=60 columns=120
```

## Builder Pattern
객체 생성 시, Builder 패턴을 사용하면 파라미터가 많을 경우 제공 상태를 일관성 있게 하고, object를 생성시킬 때, step-by-step으로 만들 수 있도록 할 수 있다.
### 작업순서
1) class 안에 중첩 static class를 생성시키고, 바깥쪽 class의 argument들을 안쪽 static class(builder class)로 옮긴다. 바깥쪽 class의 생성자는 private로 선언해서 직접적인 생성을 막는다.
2) builder class의 생성자를 public static으로 선언하고 필요한 파라메터들을 요청한다.
3) builder class에는 선택적 파라메터에 대한 setter method가 있어야한다. 그리고 선택적 인자를 설정한 후에도 같은 builder object를 리턴해야한다.
4) 마지막으로로 클라이언트 프로그램이 요청하는 object를 받을 수 있도록 build method를 만드는데, build method에서는 바깥쪽 class의 생성자가 builder 클래스의 인자를 받을 수 있도록 제공한다.

## 리액티브 프로그래밍
뭐지?

## Jsoup
Html 파서 라이브러리
### 참고
- [jsoup download](https://jsoup.org/download)
- [jsoup - 자바를 위한 BeautifulSoup (HTML parser)](http://edoli.tistory.com/95)

## 안드로이드와 테스트 왜 필요한가?
앱이 기능이 점점 추가되고 복잡해져 가면서 코드가 점점 누더기가 되어갔다. 수정에 대한 부작용을 파악하기 어려워졌고,
테스트의 필요성을 느꼈다.
[Android App과 TDD-임유진](http://egloos.zum.com/benelog/v/3017442)
## TDD 왜 쓰나? 어떻게 쓰나?
Service와 Persistence 계층
## GUI 프로그래밍의 TDD??

# 웹앱
## AMP, PWA
[구글이 제시하는 미래의 웹](https://byline.network/2016/05/1-169/)

# 안드로이드
## JRebel
JRebel,, HotSwap
[JRebel과 인스턴트-런 비교, 핫/웜/콜드 스왑 요약정리](https://academy.realm.io/kr/posts/aw204-instant-run-hot-cold-warm-swap/)

## 가속도 센서로 속도 구하기
[가속도센서](http://ko.android.wikia.com/wiki/가속도_센서)
[calculate speed from accelerometer](https://physics.stackexchange.com/questions/153159/calculate-speed-from-accelerometer)
## 용량줄이기
[application 용량 줄이기](https://www.slideshare.net/HosukShin1/application-77456003)
## navigation bar check
[stackoverflow answer](https://stackoverflow.com/questions/16092431/check-for-navigation-bar)
## JobIntentService
[Android O에서의 백그라운드 처리를 위한 JobIntentService](https://medium.com/@cwdoh/android-o에서의-백그라운드-처리를-위한-jobintentservice-250af2f7783c)
## NestedScrollView
NestedScrollView를 이용하면 ScrollView안에 ScrollView를 구현할 수 있다.
## flavorDimensions

## firebase remote config?
Firebase 원격 구성으로 클라우드에서 앱의 매개변수를 정의하고 업데이트하면 앱 업데이트를 배포하지 않고도
앱의 모양과 동작을 수정할 수 있습니다. 이 가이드에서는 앱에서 다음 단계에 따라 원격 구성을 사용하는 방법을 다룹니다.
## PreferenceScreen
xml에서 사용, CheckBoxPreference 뭥미
## Chronometer
안드로이드에서 제공하는 타이머
Handler.sendMessageDelayed 를 사용해서 표시하고 있음
## fragment commit exception
commit()은 Activity의 onSaveInstanceState() 하기전에 수행되어야 하며, 이를 어길시 java.lang.IllegalStateException: Can not perform this action after onSaveInstanceState 예외가 발생된다
화면갱신이 필요한 경우에는 commit()을 쓰지 말고 commitAllowingStateLoss()를 호출 해서 onSaveInstanceState()와 무관하게 commit를 할 수 있다.
[fragmenttransaction에 대해서..](http://www.kmshack.kr/2013/08/fragment-파헤치기-3-fragmentmanager-fragmenttransaction에-대해서/)

## pidcat
[logcat color script](https://github.com/JakeWharton/pidcat/)
## bazel
TensorFlow 안드로이드 데모를 하기 위해 필요하다
TensorFlow: 머신러닝
[tensorflow](https://tensorflow.blog/텐서플로-첫걸음/)
[Bazel 알아보기](http://limelab.tistory.com/20)
gradle 처럼 빌드 시스템, gradle보다 빠르게 빌드할 수 있다
## fullt text Search
sqlite 에서 보다 빠르게 문자열 검색
[full-text-sarch](http://stackoverflow.com/questions/29815248/full-text-search-example-in-android)
## WeakReference
메모리 관리를 위해 사용,
## android:includeFontPadding
폰트의 여백 제거
## AdapterDelegate
[JOE'S GREAT ADAPTER HELL ESCAPE](http://hannesdorfmann.com/android/adapter-delegates)
## annotation
[Between 사용 사례: APT를 활용한 Realm 마이그레이션](https://news.realm.io/kr/news/realm-meetup-vcnc-between-case-study/)
## 안드로이드 플랫폼 빌드 관련
[AndroidPlatformBuilder](https://github.com/ganadist/AndroidPlatformBuilder)
[샘플](https://android.googlesource.com/device/sample/+/android-7.1.2_r6/products/sample_addon.mk)
## DiffUtil
[RecyclerView DiffUtil로 성능 향상하기](http://www.kmshack.kr/2017/04/recyclerview-diffutil/)
## layout에 관한 이야기
layout 퍼포먼스??
LinearLayout << RelativeLayout <<<<< ConstrainLayout
### RelativeLayout vs ConstraintLayout
weight + depth 들어가면 RelativeLayout 이 더 낫고요
뷰 간 관계가 많아지면 ConstraintLayout
## webview posturl 통신
```java
Response response = okHttpClient.newCall(request).execute();
mWebView.loadDataWithBaseURL(mUrl, response.body().string();, "text/html", "utf-8", null);
```  

## Doze mode test
(https://developers-kr.googleblog.com/2015/08/testyourapponandroid60.html)
Apps that have been running foreground services (with the associated notification) are not restricted by doze.
(http://stackoverflow.com/a/33077301)
[Doze mode와 foreground서비스](https://brunch.co.kr/@huewu/3)
## 메뉴
(https://developer.android.com/guide/topics/ui/menus.html?hl=ko)
## G pad 연결
usb 연결을 mtp->ptp 로 변경해서 테스트 한다...
(http://www.clien.net/cs2/bbs/board.php?bo_table=cm_app&wr_id=12979)
## TextView 마지막 글자 안짤리게
android:textAlignment="viewEnd"
## NDK 빌드할 때... multiple target patterns. Stop.
다음 폴더를 지우면 해결된다.
프로젝트/obj/local/armeabi

## AlertDialog positiveButton dismiss 막기
alertDialog.setOnShowListener 사용 (http://stackoverflow.com/a/26087947)

## Activity 중복 실행 막기
```xml
android:launchMode="singleInstance"
android:excludeFromRecents="true"
```
SingleTask 모드
startActivity()실행시 Activity가 존재하면 Activity를 Destroy하고 재생성한다.
Activity에서 startActivityForResult() 실행시 결과를 받을수 없다.
(일반적인 어플에서 사용 불필요)

SingleInstance 모드
startActivity()실행시 ​Activity가 존재하면 재생성하지 않고 기존 Activity를 재활용한다.
Activity에서 startActivityForResult() 실행시 결과를 받을수 없다.
​(일반적인 어플에서 사용 불필요)
onStart()에서 getIntent()사용하지 말고 onNewIntent()로 받아야 한다.﻿
​
SingleTop 모드
startActivity()실행시 Activity가 존재하면 재생성하지 않고 기존의 Activity를 재활용한다.
Activity에서 startActivityForResult() 실행시 결과를 받을수 있다.

Standard 모드(기본값)
startActivity()실행시 Activity 존재여부에 상관없이 재생성하므로 동일 Activity가 여러개 될수 있다.
Activity에서 startActivityForResult() 실행시 결과를 받을수 있다.
[출처] 안드로이드사이드 - http://www.androidside.com/bbs/board.php?bo_table=B49&wr_id=145947#c_145963
## style
```xml
<item name="colorPrimary">#e0e0e0</item>
<item name="colorPrimaryDark">#a6a6a6</item>
<item name="colorAccent">#a6a6a6</item>
```
## Editable, span
### Spannable
문자열의 확장판
[Spannable 너는 뭐 하는 녀석이냐?](http://aroundck.tistory.com/300)


## MultiAutoCompleteTextView
Adpater 가 있는 상태에서 filter 를 사용하게 된다
performFiltering 에 tokenizer 를 사용함, findTokenStart로 시작위치를 찾음
performFiltering 를 상속받아서 사용하면 될듯
onEditorAction - handleDone 키보드에서 완료 버튼 선택
performCompletion(select), replaceText(keyboard)
Adapter 가 없을 때는? filter를 추가하면?

setFilter로 추가하면 단어만 남음, 알아서 필터해주나? setFilter 쓰느겍 낫남?

## DialogFragment
width, height 값을 조정하기 위해서 minWidth와 minHeight 을 사용

<<<<<<< HEAD
## 태블릿 확인
value 에 sw-600dp 에 boolean 값을 넣고  value 로 확인
```xml
// 일반
<bool name="isTablet">false</bool>
// sw600dp
<bool name="isTablet">true</bool>
```

### 참고
[Data Binding Library](https://developer.android.com/topic/libraries/data-binding/index.html)
[DataBinding-findViewById 이제 안녕~](http://gogorchg.tistory.com/entry/Android-DataBinding-findViewById-%EC%9D%B4%EC%A0%9C-%EC%95%88%EB%85%95)
[안드로이드 데이터 바인딩-SlideShare](http://www.slideshare.net/gdgkorea/databinding)

=======
## dimension 안될때
[왜 android:elevation이 안되지!](http://sunphiz.me/wp/archives/2000)
>>>>>>> 449bd35757ad4be2b3781752bc27d4036c2ae380
## Activity 에서 onCreateView 의 역할
onCreateView 너는 뭐니?
View onCreateView (String name, Context context, AttributeSet attrs)
Standard implementation of LayoutInflater.onCreateView..
### 어디서 쓰나?
Fragment 생성 시, infalte를 호출하면 Activity에서 받아서 View를 구성할 수도 있음

## gradle error 'libsWithStripDebugSymbolForDebug'
compileSdkVersion이 안 맞을 경우 발생한다.. 프로젝트에서
compliSdkVersion 19 ->compliSdkVersion 21으로 변경하니 빌드됨

## gradle error 'This version of android studio is incompatible with the gradle version used.Try disabling the instant run'
> File → Settings → Preferences dialog → Build → Execution → Deployment → Instant Run  

You can disable instant run
[answer](http://stackoverflow.com/a/35252539/6811452)

## gradle error 'org.gradle.internal.logging.LoggingManagerInternal'
gradle version 문제로 gradle-wrapper.properties 에서 gradle version을 변경하니 해결됐다.
[answer](http://stackoverflow.com/questions/38530788/solvedunable-to-load-class-org-gradle-internal-logging-loggingmanagerinternal)


## @SupressLint("NewApi")
해당 프로젝트의 설정 된 minSdkVersion 이후에 나온 API를 사용할때  warning을 없애고 개발자가 해당 APi를 사용할 수 있게 합니다.

## Palette, Swatch 활용 이미지 테마 적용
[Palette,Swatch를 활용해서 이미지의 테마색 가져오기](http://gun0912.tistory.com/67)

## OpenCV-android-sdk
### OpenCVLibrary 빌드 중 발생 에러
프로젝트명이 "OpenCV Library 2.3.13.1" 로 빌드가 안되서 발생한 에러
프로젝트명을 "OpenCVLibrary"로 변경함

## UncaughtExceptionHandler 비정상 종료 처리 + GA 전달
UncaughtExceptionHandler 를 이용해서 비정상 종료를 catch 할 수 있다.
Trhead는 발생하는 예외를 uncaughtTread를 호출하게 되어 있다 그래서 Thread의 UncaughExceptionHandler 인스터스를 Thread에 등록해서
예외 발생을 catch 할 수 있다.
ExceptionReporter는 UncaughtExceptionHandler 인터페이스를 사용하는 애로 uncaught exceptions를 GoogleAnalytics에 보고할 때, 사용한다.
```java
// Application 클래스
@Override
    public void onCreate() {
        super.onCreate();

        uncaughtExceptionHandler = new MyUncaughtExceptionHandler();
        Thread.UncaughtExceptionHandler handler = new ExceptionReporter(
                new GoogleAnalyticsUtil(this).getV3EasyTracker(),
                GAServiceManager.getInstance(),
                uncaughtExceptionHandler,
                this);
        Thread.setDefaultUncaughtExceptionHandler(handler);
    }

public class MyUncaughtExceptionHandler implements Thread.UncaughtExceptionHandler {

        @Override
        public void uncaughtException(Thread thread, Throwable ex) {
            Logger.d(TAG, "uncaughtException" + getStackTrace(ex));
            System.exit(2);
            uncaughtExceptionHandler.uncaughtException(thread, ex);
        }
    }
```
[UncaughtExceptionHandler를 이용한 앱 비정상 종료시 Log전송 및 재실행 하기](http://www.kmshack.kr/2013/03/uncaughtexceptionhandler%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%95%B1-%EB%B9%84%EC%A0%95%EC%83%81-%EC%A2%85%EB%A3%8C%EC%8B%9C-log%EC%A0%84%EC%86%A1-%EB%B0%8F-%EC%9E%AC%EC%8B%A4%ED%96%89-%ED%95%98/)
[ExceptionReporter](https://developers.google.com/android/reference/com/google/android/gms/analytics/ExceptionReporter)

## ListView Row height  
row layout 에서 height 을 셋팅했는데 앱 실행 후, 확인해보면 반영이 안됨
문제가 발생하게 된 코드,,
```java
 convertView = mInflater.inflate(R.layout.item_setting_menu, null);
```
두 번째 인자에 null 이 들어가는데 이와 관련한 내용을 찾아보면 다음과 같은 말이 있다.
LayoutInflater will automatically attempt to attach the inflated view to the supplied root.
the framework has a check in place that if you pass null for the root it bypasses this attempt to avoid an application crash.
음.. 뭔 말인지.. layoutInflater 는 rootView에  inflated view를 붙이려고 시도하는데
rootView가 null 이면 충돌을 피하기 위해 이런 시도를 bypass 한다? 어쨌든 rootView에 null 이 들어가서
row layout 의 root element 에 고정한 height 이 childView의 속성으로 변경이 된 듯하다..
이것을 다음과 같이 수정한다
수정한 코드
```java
convertView = mInflater.inflate(R.layout.item_setting_menu, parent, false);
```
원했던 결과를 얻을 수 있다.
attachToRoot 파라미턴에 대한 설명
Whether the inflated hierarchy should be attached to the root parameter? If false, root is only used to create the correct subclass of LayoutParams for the root view in the XML
### 참고
[Layout Inflation as Intended](https://possiblemobile.com/2013/05/layout-inflation-as-intended/)

## [개발에 유용한 도구들](http://www.slideshare.net/kingori/ss-68326596)
### Stetho
페이스북이 만든 킹왕짱 종합 선물세트? 크롬 브라우저의 inspect UI를 이용해 각종 정보를 조회한다
(네트워크, Sqlite DB, SharedPreference, UI)
(http://facebook.github.io/stetho/)
#### 주요 기능
- 네트워크 로깅
- 앱 내부 sqlite DB SQL 실행
- SharedPreference 조회/수정
- 커스텀 동작 수행할 수 있는 dump plugin
- javascript console
#### 기본 설정
```gradle
debugCompile 'com.facebook.stetho:stetho:1.4.1'
```
```java
Stetho.initializeWithDefaults(this)
```
adb 연결 후 크롬 브라우저에서 chrome://inspect 로 이동
#### 네트워크 로깅
```gradle
compile 'com.facebook.stetho:stetho-okhttp3:1.4.1'
```
```java
OkHttpClient.addNetworkInterceptro(new StethoInterceptor());
```
### LeakCanary
Square에서 만든 액티비티 메모리 릭 탐지 도구
(https://github.com/square/leakcanary)
### HierarchyViewer
SDK가 제공하는 뷰 성능 / 속성 조회 도구
(https://developer.android.com/studio/profile/optimize-ui.html#HierarchyViewer)
#### LayoutInspector
안드로이드 스튜디오 2.2 에 추가된, Hierarchy Viewer의 계승자

##[오픈소스 라이브러리 사용 가이드](http://www.slideshare.net/jyte/ss-68249803)

### LeakCanary
### HierarchyViewer,
### Debugger


## parcelable 인터페이스
커스텀 클래스나 오브젝트를 다른 컴포넌트에 전달하는 경우 사용
### parcelable 사용 예
parcelable을 사용하면 좀 더 쉽게 전달할 수 있다.
사용하지 않을 때,
```java
// 보내는 쪽
Intent intent = new Intent(getActivity(), MovieDetailActivity.class);
intent.putExtra(MovieDetailActivity.CARD_PROFILE, card.getProfileImage());
intent.putExtra(MovieDetailActivity.CARD_NAME, card.getName());
intent.putExtra(MovieDetailActivity.CARD_TIME, card.getUpdatedTime());
intent.putExtra(MovieDetailActivity.CARD_PICTURE, card.getPicture());
intent.putExtra(MovieDetailActivity.CARD_DESCRIPTION, card.getDescription());
intent.putExtra(MovieDetailActivity.CARD_SOURCE, card.getSource());
intent.putExtra(MovieDetailActivity.CARD_ID, card.getId());
startActivity(intent);
// 받는 쪽
String cardName = intent.getStringExtra(MovieDetailActivity.CARD_NAME);
String cardTime = intent.getStringExtra(MovieDetailActivity.CARD_TIME);
String cardProfile = intent.getStringExtra(MovieDetailActivity.CARD_PROFILE);
String cardPicture = intent.getStringExtra(MovieDetailActivity.CARD_PICTURE);
String cardDescription = intent.getStringExtra(MovieDetailActivity.CARD_DESCRIPTION);
String cardSource = intent.getStringExtra(MovieDetailActivity.CARD_SOURCE);
String cardId = intent.getStringExtra(MovieDetailActivity.CARD_ID);

return new Card.Builder()
        .id(cardId)
        .name(cardName)
        .createdTime(cardTime)
        .profileImage(cardProfile)
        .picture(cardPicture)
        .description(cardDescription)
        .source(cardSource)
        .createCard();
```
사용할 때,
```java
// 보내는 쪽
Intent intent = new Intent(getActivity(), MovieDetailActivity.class);
intent.putExtra(MovieDetailActivity.CARD_INFO, card);
startActivity(intent);
// 받는 쪽
card = getIntent().getParcelableExtra(CARD_INFO);
```
## 라이브러리
parcelable을 쉽게 해주는 parceler 라이브러리가 있음
### 참고
- [안드로이드에서 parcelable이 뭔지 자세히 설명해주세요](http://hashcode.co.kr/questions/882/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90%EC%84%9C-parcelable%EC%9D%B4-%EB%AD%94%EC%A7%80-%EC%9E%90%EC%84%B8%ED%9E%88-%EC%84%A4%EB%AA%85%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94)
- [Using Parceler](https://guides.codepath.com/android/Using-Parceler)
- [Android Parcelable 인터페이스 구현](http://thdev.net/364)


## Flavor
설정한 Flavor 파일들 중에 원본의 src와 res에 들어있는 중복되는 파일들 교체 빌드 해주는 것

## SwipeRefreshLayout
The SwipeRefreshLayout should be used whenever the user can refresh the contents of a view via a vertical swipe gesture.
```java
mSwipeRefresh.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
    @Override
    public void onRefresh() {
        // 이 부분에 리플래시 시키고 싶으신 것을 넣어 주시면 됩니다.
    }
});

// 리플래쉬 ui 종료
mSwipeRefresh.setRefreshing(false);
```

## Activity launchmode singleTask 사용
Activity:launchmode 중 sigleTask 와 singleInstance는 한 개의 instance 만을 가질 수 있다.
그래서 항상 stack의 root를 차지한다.
### SingleTask 사용 예
Activity Stack이 다음의 순서로 쌓인 상황에서 `A->B->C->D` 특정 이벤트가 발생했을 때, A로 가려고 하는 경우 사용
A Activity에 sigleTask option 을 추가하고, 이벤트가 발생했을 때, startActivity를 호출한다.
### singleTask와 sigleInstace 차이
sigleTask는 다른 activity 들이 자신의 instance 위에 쌓이는 것을 허락해준다.
singleInstance는 다른 activity를 자신의 task 안에 포함시키 않는다.
### 참고
[Android moving back to first activity on button click](http://stackoverflow.com/a/2776875/6811452)
[간단하게 Activity stack 처리하는 방법](http://mokiprogramming.blogspot.kr/2014/01/activity-stack.html)

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
Map<스트링, String> getKeyValueFromStringArray(Context ctx) {
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
