# 무선으로 인터넷 공유
## 드라이버 확인
netsh wlan show drivers // 호스트된 네트워크 지원
## 호스트 네트워크 셋팅
netsh wlan set hostednetwork mode=allow ssid=testtest key=12345678 keyUsage=persistent
## 호스트 네트워크 시작
netsh wlan start hostednetwork
## 호스트 네트워크 종료
netsh wlan stop hostednetwork

## 인터넷 공유
제어판 > 네트워크 연결 > 이더넷 오른쪽 클리 후, 속성 > 공유 탭에서 "다른 네트워크 사용자가..." 체크 >
홈네트워킹 아래쪽에 "로컬 영역 연결* 숫자" 선택 > 확인

## 참고
- [호스트된 네트워크를 시작할 수 없습니다 오류 해결](http://sergeswin.com/1045)
- [호스트된 네트워크가 없을 때](http://answers.microsoft.com/ko-kr/windows/forum/windows_10-hardware/%EC%9C%88%EB%8F%84%EC%9A%B0%EC%A6%88-101607/f24ad1e9-e465-43a5-b9c2-b945daa66a90?tm=1471015027177)
- [드라이버 롤백](http://nancom.tistory.com/256)
- [무선인터넷 공유 방법](http://sergeswin.com/1031)
## 스마트폰을 네트워크망에 물려 테스트할 때,
### 네트워크 설정 변경
위와 같이 wlan 호스트 설정 후,
네트워크 연결 설정 > 이더넷 속성 > 공유 > 다른네트워크 사용자 허용 (생성한 네트워크를 허용해준다.)
### 호스트 변경
C:\Windows\System32\drivers\etc\hosts 파일을 열어
nslookup [address] 명령 실행해서 나오는 응답 address와 aliases를 hosts 파일에 추가해준다.
