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

## 참고
[변경사항](https://developer.android.com/about/versions/oreo/android-8.0-changes.html?hl=ko#atap)
[jobinentservice](https://medium.com/til-kotlin-ko/android-o에서의-백그라운드-처리를-위한-jobintentservice-250af2f7783c)
