

```
mysql -h localhost -P 3306 --protocol=tcp -u root -p
```


https://qastack.kr/programming/33001750/connect-to-mysql-in-a-docker-container-from-the-host
Docker MySQL 호스트가 올바르게 실행중인 경우 로컬 컴퓨터에서 연결할 수 있지만 다음과 같이 호스트, 포트 및 프로토콜을 지정해야합니다.
mysql -h localhost -P 3306 --protocol=tcp -u root
3306을 Docker 컨테이너에서 전달한 포트 번호로 변경합니다 (귀하의 경우 12345).
Docker 컨테이너 내에서 MySQL을 실행하고 있기 때문에 소켓을 사용할 수 없으며 TCP를 통해 연결해야합니다. mysql 명령에서 "--protocol"을 설정하면 변경됩니다.