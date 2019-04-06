## Doze mode test
(https://developers-kr.googleblog.com/2015/08/testyourapponandroid60.html)
Apps that have been running foreground services (with the associated notification) are not restricted by doze.
(http://stackoverflow.com/a/33077301)
[Doze mode와 foreground서비스](https://brunch.co.kr/@huewu/3)

## Doze 모드
안드로이드 6.0 마쉬멜로우에 새롭게 추가된 절전모드
일정시간동안 움직이지 않는 경우, 디바이스는 Doze 모드에 진입하고 앱들은 배터리 소모가 많은 기능, 네트워크 연결, GPS 스캔 등을 활용할 수 없음..
### 참고
- [Doze와 Foreground Service](https://brunch.co.kr/@huewu/3)

## state
IDLE 과 MAINTENANCE 단계가 DOZE 모드
IDLE_PENDING
IDLE
IDLE_MAINTENANCE
SENSING
LOCATING
ACTIVE
INACTIVE


# 유휴상태
```
$ adb shell dumpsys battery unplug
$ adb shell dumpsys deviceidle step
$ adb shell dumpsys deviceidle  // 상태 확인
```
원래대로
```
$ adb shell dumpsys battery reset
```

# 대기상태
대기모드
```
$ adb shell dumpsys battery unplug
$ adb shell am set-inactive <packageName> true
```
활성화
```
$ adb shell am set-inactive <packageName> false
$ adb shell am get-inactive <packageName> // 상태확인
```


# 참고
[android m 리뷰](https://medium.com/marojuns-android/좀-더-생각해본-android-m-리뷰-13fbb98c9a87)
