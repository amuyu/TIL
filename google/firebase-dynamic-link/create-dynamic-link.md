# 동적 링크 만들기 


## REST API 로 동적 링크 만들기
API 및 클라이언트 SDK 로 만든 짧은 동적 링크는 Firebase Console 에 표시되지 않는다.
이 API 는 긴 동적 링크 또는 동적 링크 매개변수가 들어 있는 객체를 위하며 다음 예시와 같은 URL 을 반환한다.
```
https://example.page.link/WXYZ
```
마케팅 사용 사례인 경우, Firebase Console 에서 직접 링크를 만들어야 한다.

### 짧은 동적 링크 만들기
#### 긴 링크로 짧은 링크 만들기
Firebase Dynamic Links API 를 사용해서 긴 동적 링크를 축약할 수 있다.
```
POST https://firebasedynamiclinks.googleapis.com/v1/shortLinks?key=api_key
Content-Type: application/json

{
   "longDynamicLink": "https://example.page.link/?link=https://www.example.com/&apn=com.example.android&ibi=com.example.ios"
}
```

#### 긴 동적 링크 만드는 방법
```
https://your_subdomain.page.link/?link=your_deep_link&apn=package_name[&amv=minimum_version][&afl=fallback_link]
```
`link` 는 앱에서 열리는 링크
매개변수는 [이곳](https://firebase.google.com/docs/dynamic-links/create-manually?hl=ko) 에서 확인, 종류가 많음

android 와 ios 둘다 동작하는 동적 링크
```
https://example.page.link/?link=https://www.example.com/someresource&apn=com.example.android&amv=3&ibi=com.example.ios&isi=1234567&ius=exampleapp
```
URL 디버깅
디버그 매개변수를 추가하면 동적 링크를 디버깅할 수 있습니다.
```
https://example.page.link/?link=https://www.example.com&d=1
https://example.page.link/WXYZ?d=1
```

#### 매개변수로 짧은 링크 만들기
```
POST https://firebasedynamiclinks.googleapis.com/v1/shortLinks?key=api_key
Content-Type: application/json

{
  "dynamicLinkInfo": {
    "domainUriPrefix": "https://example.page.link",
    "link": "https://www.example.com/",
    "androidInfo": {
      "androidPackageName": "com.example.android"
    },
    "iosInfo": {
      "iosBundleId": "com.example.ios"
    }
  }
}
```
`dynamicLinkInfo` 매개변수 사양 [이곳](https://firebase.google.com/docs/reference/dynamic-links/link-shortener?hl=ko) 을 참고 
```
{
  "dynamicLinkInfo": {
    "domainUriPrefix": string,
    "link": string,
    "androidInfo": {
      "androidPackageName": string,
      "androidFallbackLink": string,
      "androidMinPackageVersionCode": string
    },
    "iosInfo": {
      "iosBundleId": string,
      "iosFallbackLink": string,
      "iosCustomScheme": string,
      "iosIpadFallbackLink": string,
      "iosIpadBundleId": string,
      "iosAppStoreId": string
    },
    "navigationInfo": {
      "enableForcedRedirect": boolean,
    },
    "analyticsInfo": {
      "googlePlayAnalytics": {
        "utmSource": string,
        "utmMedium": string,
        "utmCampaign": string,
        "utmTerm": string,
        "utmContent": string,
        "gclid": string
      },
      "itunesConnectAnalytics": {
        "at": string,
        "ct": string,
        "mt": string,
        "pt": string
      }
    },
    "socialMetaTagInfo": {
      "socialTitle": string,
      "socialDescription": string,
      "socialImageLink": string
    }
  },
  "suffix": {
    "option": "SHORT" or "UNGUESSABLE"
  }
}
```




# ref
[reference](https://firebase.google.com/docs/dynamic-links/rest?hl=ko)