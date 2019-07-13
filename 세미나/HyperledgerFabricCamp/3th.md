
# 이메일
smkkang2@gmail.com
net0310@gmail.com

# 디버깅
메일로 물어보면 디버깅하는 방법 알려줌

## mount
sudo mkdir data
sudo fdisk -l
sudo mkfs.ext4 /dev/nvme0n1
sudo mount /dev/nvme0n1 /data/
sudo mkdir -p /etc/docker
sudo vi /etc/docker/daemon.json
```json
{
    "data-root":"/data/docker"
}
```
sudo apt-get update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
sudo apt-get install docker-compose

sudo apt-get install golang-go
GOROOT, GOPATH 설정
go get -u github.com/hyperledger/fabric
go get -u github.com/hyperledger/fabric-samples
go get -u github.com/hyperledger/fabric-sdk-node

// 번외편
sudo chown -R ubuntu.ubuntu /data 
docker run hello-world


## library
node.js
docker
fabric

# SDK
현재는 node.js 가 대세
앞으로는 java 가
하이퍼레저 캘리퍼?

# 필수 라이브러리 설치
- Node.js / NVM
- Gulp

# 실습
## fabric-sdk-node
build/test.js
```js
// fabric end-to-end test
gulp.task('run-end-to-end', (done) => {
	const tasks = ['clean-up', 'docker-clean', 'pre-test', 'ca', 'compile', 'run-tape-e2e'];
	runSequence(...tasks, done);
});
```

## fabric-samples/balnace-transfer
loginRequest
name, orgname
Readme.md 에 있는 script 실행

`2-of` 이기 때문에 org 1, 2 에 install? 이 필요하다
```json
{
	identities: [{
			role: {
				name: 'member',
				mspId: 'Org1MSP'
			}
		},
		{
			role: {
				name: 'member',
				mspId: 'Org2MSP'
			}
		}
	],
	policy: {
		'2-of': [{
			'signed-by': 0
		}, {
			'signed-by': 1
		}]
	}
}
```

instantiate 는 channel 에 종속? chaincode 는 channel 에 올라간다?


github//
hyperledger study 02



# Q&A ? TIP?
- 엔도서에 의해서 chaincode 실행
- CA 개수가 의미하는바는? ORG 와 수를 일치
- CA 를 쓴다는 의미는? revoke 때문에.. 
- core.yaml : legder.state.stateDatabase: goleveldb => couchdb 이 부분은 docker_compose 에서 속성으로 override 해서 설정 변경
- 하이퍼레저는 권한 기반이기 때문에 권한에 대한 architecture 에 대한 이해가 필요하다.
- 한 서버가 아니라 multi로 구성을 한다면 swarm 이나 kubernates 를 사용해야 한다. - swarm 도 좋아졌다
- CA 가 사용하는 default db - file db hsql? db
- fabric-samples/balance-trasnfer 처럼 사용하면 안된다 -> context switch 하는 방식으로 사용해야 한다.
- fabric/sampleconfig/ 에 core.yaml peer.yaml 등을 볼 수 있다.
- discovery 기능, gossip? target 의 peer 를 안날리면 전체로 쏜다 요런 기능을 사용하면 peer cpu 사용량 85% 이상이면 호출안하도록
- batchtimeout, maxsize, maxcount
    ex) 0.5s, 1000개 => block 을 보면 500개가 안된다. 그렇다면 분산환경이 구축이 안됐다.
- matric api, graph 로 monitor 관리를 할 수 있다.
- raft 빠르다. fabric-samples/first-network ?? orderer 가 5개? reader?는 홀수 (2n + 1), mininum 은 3대
- world state db 사용? 
    leveldb : 성능이 빠르다. 
    couchdb : 페이징 기능, cluster로 돌리고, map reducer? 다른 분야와의 융합 빅데이터?
- 성능문제? 
    api server 성능, 엔도서 정책, docker 설정을 봐야(file system, network adapter,,)
    zookeeper, kafka : ssd
    엔도서 : cpu 가 높아야
- v2.0 에서 token 컨소시엄 구성에 고민이 필요

request createChannel => orderer => channel msp 생성

# 과제
generator-express-no-stress 설치하고 swagger 화면 까지 스샷




# 책
Hands On Blockchain with Hyperledger