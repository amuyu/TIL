

less 로 log 볼 때, `-R` 옵션을 넣으면 `ESC` 없이 볼 수 있다.

ip a
vmstat 5 # stat 5초 간격으로 호출
time curl 192.168.0.6:9080/admin/chain # time 명령으로 하면 응답 시간 확인 가

netstan -lntp
ps -ef |grep NetworkMana
ps -ef |grep firewall

hostname # hostname 알아오는 명령

도메인 네임 서버의 정보 확인
dig q-test.zzeung.id +short
host q-tet.zzeung.id
nslookup q-test.zzeung.id

apt install dnsutils
nslookup

mac network
lsof -iTCP -sTCP:LISTEN -n -P


netstat -antpo |grep 10.8.10.230
watch -n 1 "netstat -antpo |grep 10.8.10.230"

# 파일만
find ./ -type f -exec chmod -v 755 {} \;
# 폴더만
find ./ -type d -exec chmod -v 755 {} \;

# DS_Store 지우기
find ./ -name ".DS_Store" -depth -exec rm {} \;


# 용량 확인
du -sh kend_home