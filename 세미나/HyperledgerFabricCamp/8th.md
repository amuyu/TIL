# Multi host?
fabric-bcp, kttns_5G, gr.kt.com
docker_compse.xml -> extra_hosts 에서 설정 가능
swarm 네트워크를 통해서 hots 설정없이(hostname 과 ip 설정없이) orderer 나 peer 이름으로 호출 가능
node 별로 compose 구성

# first-network 연동?
에러의 detail 한 내용을 보려면? - intellij 에서
network 를 up 한 후, 
api server 에서 `npm run compile` 
build 디렉토리에서 main.js 를 디버그 모드로 실행 가능
transaction.js 에서 sendTransactionProposal 의 result 를 확인
failed to connect before the deadline URL:grpcs:// ... => 인증서가 틀릴 때 발생하는 에러 메시지
docker ps -a 해서 network 확인하고 그 후, 어떤 인증서를 사용하는지 알려면?
docker inspect 라는 명령어로 출력되는 정보로 인증서 위치를 확인할 수 있음
인증서 복사
개인키 갱신 - network-config 파일 갱신
npm run compile 로 빌드 파일 다시 생성하고
디버그 다시 실행
incorrect number of arguments. expecti ... => chaincode container 에서 발생하는 에러
arguments 를 수정하려면 어떤걸 참고해야하는가? 코드 확인은?
firstnetwork... script...utils 에서 script 명령을 참고해서 argument 를 수정


./byfn -m up -l java
로 하면 java chaincode 가 올라감

# multi channel
configtxgen.sh - multichannel 생성
couchdb 확인
channel 별 원장, 연관관계 없음

천만건 trasnaction 발생
ledger 1 개에 쓰는 것보다 ledger 3개에 쓰는 게 성능이 좋다.
문제점1? 어느 원장에 어느 데이터가 있는지 모른다.
중간에 cach 서버를 두고 어떤 key 가 어떤 채널에 있는지 들고 있는다
문제점2? 서로 다른 원장에 있는 데이터에 대해서 이중지불? 문제가 발생할 수 있다.
distribute lock 처리할 수 있는 방법이 필요
문제점3? 이게 블록체인인가?





# tip
hlf-clinet.js
transaction.js
getAdminClinetForOrg() => 수정포인트 현재는 매번 객체를 생성하고 있음
현재 코드는 method 를 실행할 때마다 호출하기 때문에 singleton 느낌으로 셋팅하도록 수정



# raft
etcd 에 있는 raft 코드를 그대로 가져옴 -> 1.4.1


# bug report
- 1.4.2 version 에서 raft 버그 현재 8/3 버그 해결 안됨
peer 가 하나 죽으면 전체 노드들이 다 죽는다.


# multi orderer
https://github.com/simon0210/hyperledger-study01/blob/master/network/multi/docker-compose.yml

# 할일
- configtxgen.sh, configtx.yaml 을 해보세욤

# 문의
`특별한5기` 를 붙여주세요
net0310@gmail.com

# 잡담
강사님 회사 아직 tf temporary - hyperledger
클레이튼?
블록체인 사업 , 1회말
- fabric : private token, 어음 용도, 
eos 기술력 높이 평가? 시장 평가? 
neowiz 가 eos 소스코드 분석? 기술적으로 욕했다?
docker? 