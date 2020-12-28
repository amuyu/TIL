

# 스프링 시작할 때 hostname 가져오는데 시간 오래 걸리는 확인할 거 
로그 메시지 
```
InetAddress.getLocalHost().getHostName() took 10013 milliseconds to respond. Please verify your network configuration.
```
해결 방법?
```sh
hostname
# output: bc-icon-1
# /etc/hosts 에 다음처럼 추가한다.
vi /etc/hosts
127.0.0.1 localhost bc-icon-1
```