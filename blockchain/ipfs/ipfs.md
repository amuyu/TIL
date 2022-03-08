
# IPFS (InterPlanetary File System) - `Protocol Labs`
HTTP Web이 느리다? 라즈비안OS 다운로드하는데 http는 너무 느려서 토렌트를 이용하는 것을 추천한다.
IPFS는 뭐지? 탈중앙화 웹이다, IPFS를 통해 우리는 인터넷 상에 존재하는 모든 파일을 블록체인 상에 올릴 수 있다.
파일코인의 초기 모델

## 특징
IPFS 분산된 P2P 파일 시스템
고용량의 파일을 빠르고 효율적으로 전달할 수 있다
IPFS 상에 업로드된 파일의 이름은 영원히 기록된다? 왜? 블록체인이야?
파일 버전 관리(Git) 가능
pinning? 고정? 지키고 싶은 파일을 지켜낼 수 있다? 데이터 유지

개인 정보를 보호하는 데이터 실드?


## How IPFS works
각각의 파일은 여러 블록으로 이루어져 있음, 블록은 해시로 표현 | 블록체인이네? 
모든 파일 이름을 데이터베이스에 저장 동일 파일의 중복 배제, 파일의 버전 정보를 트래킹
각 노드는 본인이 관심있는 파일만 저장소에 보관
네트워크에서 파일을 찾기 위해서는 파일명을 조회하고 해당 파일을 갖고 있는 노드에 물어보면 된다
 

## stack
- DHT -> 네트워크 상의 콘텐츠를 빠르고 정확히 검색할 수 있다.
    - contents-addresssing
- Bittorrent 
- Git(merkle DAG)

## filecoin
파일코인은 ipfs의 인센티브 레이어입니다.
- Storage
- retrieval



## etc
ipfs와 filecoin을 창조한 protocol labs의 juan benet

# tutorial
리눅스에서만 되나?
ubuntu 16.04.6 LTS
ipfs docker는 없나?
https://hub.docker.com/r/ipfs/go-ipfs/
https://docs.ipfs.io/how-to/run-ipfs-inside-docker/#set-up
pin 은 뭐지?


```
# 저장소 초기화
ipfs init
# readme
ipfs cat /ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv/readme
# usage example
ipfs cat /ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv/quick-start
# 데몬 실행
ipfs daemon
# file upload
ipfs add first.html
```

webui : http://localhost:5001/webui


# ipfs 테스트넷은 없나?
https://ipfs.infura.io:5001/api/v0
https://infura.io/docs/ipfs : 계정 등록해서 사용

# 로컬 테스트
http://localhost:8080/ipfs/QmZVunZYPKFVGqv2GnSAsVn576vr9spZpq8AZypHzvRgeD?filename=jetbrains-toolbox-1.22.10970.dmg
pin은 뭔가요?

# ipfs 와 블록체인
https://docs.ipfs.io/how-to/mint-nfts-with-ipfs/#how-minty-works
https://docs.ipfs.io/how-to/best-practices-for-nft-data/#types-of-ipfs-links-and-when-to-use-them

# nft 와 ipfs
https://medium.com/scrappy-squirrels/tutorial-nft-metadata-ipfs-and-pinata-9ab1948669a3

ipfs 에서 말하는 nft
http 주소를 사용하면 `https://cloud-bucket.provider.com/my-nft.jpeg` 누구나 컨텐츠를 가져올 수 있다.
NFT를 만들 때와 동일한 my-nft.jpeg 인지 보장할 수 있는 방법은 없다. 서버 소유자가 언제든지 다른 것으로 교체할 수 있다.
ipfs에 데이터를 추가하면 CID가 생성되고 ipfs 데이터에 연결되기 때문에 누구나 콘텐츠를 교체하거나 변경할 수 없다.

Minty : NFT를 가지고 놀 수 있음
https://docs.ipfs.io/how-to/mint-nfts-with-ipfs/#minty

# caver를 이용한 ipfs 사용
js
https://medium.com/klaytn/caver%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-ipfs-%EC%82%AC%EC%9A%A9%EB%B2%95-4889a3b29c0b
```
caver.ipfs.setIPFSNode('IPFS Node URL', 5001, true)
```

# 네트워크
[로컬 환경에서 IPFS Private 네트워크 실행](https://miiingo.tistory.com/205)
[ipfs cluster 로 private ipfs network 구성하기](https://hihellloitland.tistory.com/89)

# 노드 간 파일 교환
https://docs.ipfs.io/how-to/exchange-files-between-nodes/#prerequisites
contents provider, peering
https://docs.ipfs.io/how-to/peering-with-content-providers/#content-provider-list

# 유명한 노드?
pintata
protocol labs

# configure
* 4001 : 다른 노드와 통신
* 5001 : API 서버
* 8080 : 게이트웨이 서버
```
ipfs config Addresses.API /ip4/127.0.0.1/tcp/5001
ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001
ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080

ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["http://localhost:3000", "http://127.0.0.1:5001", "https://webui.ipfs.io"]'
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "POST"]'
```

# gateway
웹을 통해 ipfs에 저장된 콘텐츠를 이용할 수 있다.
ipfs.io 게이트웨이를 사용하면 인터넷 사용자가 IPFS 네트워크에서 제3자가 호스팅하는 데이터에 액세스하고 볼 수 있습니다. 
ipfs.io 게이트웨이는 개발자가 IPFS를 기반으로 구축할 수 있도록 Protocol Labs에서 실행하는 커뮤니티 리소스입니다.
일종의 호스팅 서비스?

IPFS 노드가 로컬에서 실행될 때 게이트웨이가 포함됩니다 http://127.0.0.1:8080 = 로컬 서비스로 설치된 게이트웨이
https://docs.ipfs.io/concepts/ipfs-gateway/#gateway-providers
* protocol labs : https://ipfs.io
* third-part : https://cf-ipfs.com
list public-gateway:
https://ipfs.github.io/public-gateway-checker/


# aws 에서 ipfs 돌려보기
* local에 nginx 돌려서 localhost:5001 -> aws
* settings = api address 에 /ip4/{aws_ip}/tcp/5001 설정
```
/ip4/0.0.0.0/tcp/5001
```

# ipfs 삭제는?
삭제해도 받아진다?
다시 올려도 cid는 똑같다

# inspect
정보를 보자 
Links? 나눠서 저장?
ipfs 게이트웨이로 보기 -> 다운로드?

# aws 에 ipfs 돌리기
1. 인스턴스 올리기
2. docker 설치
```
# docker 설치
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo amazon-linux-extras install docker-compose

# docker-compose 설치
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compoe
// logout
service docker restart
docker-compose -v
```


# ipfs 다운로드는 어떻게?
ipfs/ipget download
go get 으로 다운로드하면 module 로 받게된다
```
export GO111MODULE=off
```


# ref
[IPFS 이해하기 1부](https://medium.com/@kblockresearch/8-ipfs-interplanetary-file-system-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1%EB%B6%80-http-web%EC%9D%84-%EB%84%98%EC%96%B4%EC%84%9C-ipfs-web%EC%9C%BC%EB%A1%9C-46382a2a6539)
[IPFS 이해하기 2부](https://steemit.com/kr/@kblock/27-ipfs-interplanetary-file-system-2-ipfs-filecoin)
[filecoin](https://docs.filecoin.io/)
[ipfs tutorial](https://hihellloitland.tistory.com/62)
[ipfs-http-client-js](https://dev.to/dabit3/uploading-files-to-ipfs-from-a-web-application-50a)
[블록체인과 IPFS를 이용한 안전한 데이터 공유 플랫폼 - 개념과 리뷰 시스템](https://medium.com/curg/%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EA%B3%BC-ipfs%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%95%88%EC%A0%84%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B3%B5%EC%9C%A0-%ED%94%8C%EB%9E%AB%ED%8F%BC-17bf42efbb8)
[ipfs peer setting](https://medium.com/textileio/tutorial-setting-up-an-ipfs-peer-part-i-de48239d82e0)