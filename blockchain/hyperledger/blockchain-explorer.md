# version
v.0.3.9.4

# 익스플로러
블록, 트랜잭션, 노드, 체인코드의 정보를 알 수 있습니다.
익스플로러에서는 PostgreSQL 을 사용, 블록 정보 및 트랜잭션 이벤트들을 데이터베이스에 저장한 후, 저장된 데이터 활용

## dependencies
- postgresql
- jq
- nodejs

## How to Run
1. Clone repository
```bash
git clone https://github.com/hyperledger/blockchain-explorer.git
```

2. Database
postgreSQL 설치가 필요하다.
explorerconfig.json 에서 database 관련 설정을 할 수 있다.
postgre 관련 ip, port 와 database, user 정보를 셋팅한다.
database 와 user 정보는 `createdb.sh` 명령으로 추가된다.
```json
"postgreSQL": {
    "host": "127.0.0.1",
    "port": "5432",
    "database": "fabricexplorer",
    "username": "hppoc",
    "passwd": "password"
}
```
db 설정을 변경 한 후,
`blockchain-explorer/app/persistence/fabric/postgreSQL/db` 에서 `createdb.sh` 를 실행한다.
postgre 가 docker 일 경우, 스크립트를 수정한다.

local 에서는 postgresql 을 docker 를 사용해서 설치했고 `createdb-docker.sh` 를 실행한다.

3. Netowrk Setup
hyperledger network 를 실행해야 한다.
README.md 에는 BYFN 을 기준? 으로 설명하고 있다.

`docker-compose.yaml` 에서 
`CORE_PEER_GOSSIP_BOOTSTRAP` 와 `CORE_PEER_GOSSIP_EXTERNALENDPOINT` 를 각 peer 마다 
셋팅해야 explorer 에서 모니터링이 가능하다.

`balance-transfer` 를 기준으로 보면 `testAPI` 를 호출한 후부터 explorer 실행이 가능하다.


operation service 를 위한 상세 설정은 다음을 참고한다.
[config-guide](https://github.com/hyperledger/blockchain-explorer/blob/master/CONFIG-OPERATIONS-SERVICE-HLEXPLORER.md)
아마도 프로메테우스 등의 연동이 필요한 경우에 셋팅일 듯하다.

4. network-profile 수정
app/platfrom/fabric 에서 network 정보를 입력한다.
config.json 은 다음과 같다.
```json
{
    "network-configs": {
        "first-network": {
            "name": "firstnetwork",
            "profile": "./connection-profile/first-network.json",
            "enableAuthentication": false
        }
    },
    "license": "Apache-2.0"
}
```

firstnetwork 의 실제 네트워크 정보는 app/platform/fabric/connection-profile/first-network.json
에 있다. 해당 폴더 네이밍은 아마도 fabric-client 의 영향일 듯하다.
해당 json 에서 3번에서 실행한 network 의 정보 base 로 fabric-path 와 인증서를 수정한다.
> hyperledger 강의 들을 때는 json 보다는 Yaml 설정을 이용하라고 했던거 같은데
> client sdk 사용법에 대한 정리도 필요할 듯 하다.


5. build explorer
`./main.sh install` : to install, run tests, and build project
`./main.sh clean` : to clean the /node_modules, client/node_modules client/build, client/coverage, app/test/node_modules directorieso

node_modules install 의 경우 python 2.7.x 이어야 install 이 됨....

6. run
`./start.sh` 를 실행한다.

## swagger
실행 후, `api-docs` 에 접속하면 swagger 에 접속할 수 있음
http://localhost:8080/api-docs


# docker-compose 파일?
block-explorer 프로젝트 docker-compose 에 프로메테우스와 그라파나도 설정이 있음
