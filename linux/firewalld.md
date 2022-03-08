# filrewall
https://www.lesstif.com/system-admin/linux-unix/rhel-centos-tips-tricks/rhel-centos-7-firewalld


# firewalld 중지
```
systemctl stop firewalld
systemctl mask firewalld
```
출처: https://jiniweb.tistory.com/86 [마음 가는 대로]


# 조회
```
# 방화벽 실행 여부 확인
sudo firewall-cmd --state
# zone 출력
firewall-cmd --get-zones

# 기본 존 출력
firewall-cmd --get-active-zones
# 사용 가능한 서비스/포트 출력하기
firewall-cmd --list-allfirewall-cmd --list-all
# 사용 가능한 모든 서비스/포트 목록을 출력
firewall-cmd --zone=public --list-all
# 서비스 추가 제거 ZONE 옵션이 없으면 기본 존에 추가 또는 삭제
firewall-cmd --add-service=ftp
firewall-cmd --remove-service=ftp
firewall-cmd --add-port=21/tcp
firewall-cmd --remove-port=21/tcp
firewall-cmd --zone=public --add-service=ftp
firewall-cmd --zone=public --remove-service=ftp
firewall-cmd --zone=public --add-port=21/tcp
firewall-cmd --zone=public --remove-port=21/tcp
```

# 방화벽 허용
https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/securityrules.htm
```
sudo firewall-cmd --zone=public --permanent --add-port=1521/tcp								
sudo firewall-cmd --reload
```

