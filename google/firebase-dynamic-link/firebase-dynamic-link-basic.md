# Firebase Dynamic Links
ios 또는 android 에서 dynamic link 를 연 사용자를 native app contents 로 이동시킬 수 있다.
데스크톱 브라우저에서 열었다면 웹사이트의 해당 콘텐츠로 안내할 수 있다.
앱 설치 여부에 따라 적절히 작동한다. 앱을 설치하지 않은 사용자는 앱을 설치하는 화면으로 안내하고
앱을 설치하고 시작하면 최초 링크에 그대로 액세스 할 수 있다.

## 기본원리
사용자가 동적 링크 중 하나를 열었는데 앱이 설치되지 않은 경우, 개발자가 다른 작업을 지정한 것이 아니라면 
사용자는 play 스토어 또는 app store 로 이동하여 앱을 설치한 다음 앱이 열린다.
그런 다음 앱에 전달된 링크를 가져와 앱에 맞게 딥 링크를 처리할 수 있다.
> 아마 preview 페이지를 지정한 경우, 앱 설치 후, 전달 된 링크를 받을 수 없다?

## 커스텀 링크 도메인
자체 도메인 이름을 사용해서 동적 링크를 만들 수 있다.

## 사용 사례

### 웹 사용자를 앱 사용자로 전환
웹 사이트의 각 페이지에서 동적 링크를 동적으로 생성한다.
```
<a href="https://example.page.link/?link=https://www.example.com/content?item%3D1234&apn=com.example.android&ibi=com.example.ios">Open this page in our app!</a>
```

## ref
[google reference](https://firebase.google.com/docs/dynamic-links?hl=ko)