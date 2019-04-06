# oreo 변경 사항
백그라운드 실행 제한
	앱이 유휴 상태인 경우 백그라운드 서비스의 사용이 제한된다.
	명시적 브로드캐스트
	백그라운드 서비스 생성 허용, context.startForegroundService, startForeground
	JobScheduler 사용
백그라운드 위치 제한
알림채널
네트워킹
	HttpsURLConnection 안전하지 않은 TLS/SSL 버전 대체를 수행하지 않는다.
보안
식별자
경고창
	SYSTEM_ALERT_WINDOW  TYPE_SYSTEM_ERROR 사용 안됨
	TYPE_APPLICATION_OVERLAY 로 변경
콘텐츠 변경 알림
	ContentResolver.notifyChange()
미디어
	모든 오디오 관련 API 는 AudioAttributes 사용
	사용자가 전화를 받을 때, 통화하는 동안 활성 미디어 스트림 음소거
NotificationManager.startServiceInForeground()

## 암식적 broadcast 예외
https://developer.android.com/guide/components/broadcast-exceptions

## [안드로이드/Android]8.0 오레오 알림채널(Notification Channel) 대응하기
// http://gun0912.tistory.com/77
알림에 채널 생성
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
	android.app.NotificationManager notificationManager = (android.app.NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE); NotificationChannel channelMessage = new NotificationChannel("channel_id", "channel_name", android.app.NotificationManager.IMPORTANCE_DEFAULT); channelMessage.setDescription("channel description");
	channelMessage.enableLights(true);
	channelMessage.setLightColor(Color.GREEN);
	channelMessage.enableVibration(true);
	channelMessage.setVibrationPattern(new long[]{100, 200, 100, 200});
	channelMessage.setLockscreenVisibility(Notification.VISIBILITY_PRIVATE);
	notificationManager.createNotificationChannel(channelMessage);
}
```
알림에 채널 지정하기
```java
Notification.Builder builder = new Notification.Builder(context, channel)
        .setContentTitle(title)
        .setContentText(body)
        .setSmallIcon(getSmallIcon())
        .setAutoCancel(true);
```
## Heads-up Notifications
setPriority(NotificationManager.IMPORTANCE_HIGH)
// https://stackoverflow.com/a/29949603/6759520
// https://developer.android.com/guide/topics/ui/notifiers/notifications#Heads-up


## 참고
[변경사항](https://developer.android.com/about/versions/oreo/android-8.0-changes.html?hl=ko#atap)
[jobinentservice](https://medium.com/til-kotlin-ko/android-o에서의-백그라운드-처리를-위한-jobintentservice-250af2f7783c)
[노티피케이션 채널](http://g-y-e-o-m.tistory.com/73)
