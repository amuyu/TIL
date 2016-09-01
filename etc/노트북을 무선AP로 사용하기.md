# 무선으로 인터넷 공유
## 호스트 네트워크 셋팅
netsh wlan set hostednetwork mode=allow ssid=testtest key=12345678 keyUsage=persistent
## 호스트 네트워크 시작
netsh wlan start hostednetwork
## 호스트 네트워크 종료
netsh wlan stop hostednetwork

## 참고
- [호스트된 네트워크를 시작할 수 없습니다 오류 해결](http://sergeswin.com/1045)
- [호스트된 네트워크가 없을 때](http://answers.microsoft.com/ko-kr/windows/forum/windows_10-hardware/%EC%9C%88%EB%8F%84%EC%9A%B0%EC%A6%88-101607/f24ad1e9-e465-43a5-b9c2-b945daa66a90?tm=1471015027177)

## 스마트폰을 네트워크망에 물려 테스트할 때,
### 네트워크 설정 변경
위와 같이 wlan 호스트 설정 후, 
네트워크 연결 설정 > 이더넷 속성 > 공유 > 다른네트워크 사용자 허용 (생성한 네트워크를 허용해준다.)
### 호스트 변경
C:\Windows\System32\drivers\etc\hosts 파일을 열어 
nslookup [address] 명령 실행해서 나오는 응답 address와 aliases를 hosts 파일에 추가해준다.






