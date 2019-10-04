
# couchdb
자산을 JSON으로 모델링하고 CouchDB를 사용하는 경우 chaincode 내의 CouchDB JSON 쿼리 언어를 사용하여 체인 코드 데이터 값에 대해 복잡한 리치 쿼리를 수행 할 수도 있습니다.

CouchDB는 피어와 함께 별도의 데이터베이스 프로세스로 실행되므로 설정, 관리 및 작업과 관련하여 추가 고려 사항이 있습니다. 
기본 임베디드 LevelDB로 시작하는 것을 고려해 볼 수 있으며 추가로 복잡한 리치 쿼리가 필요한 경우 CouchDB로 이동하십시오. 
체인 코드 자산 데이터를 JSON으로 모델링하는 것이 좋습니다. 그러면 나중에 필요할 경우 복잡한 리치 쿼리를 수행 할 수 있습니다.

> couchdb 는 별도의 설정, 관리 등이 필요하다. 하지만 복잡한 리치 쿼리가 필요한 경우 couchdb 를 사용해라
> couchdb의 경우, 코드 자산 데이터를 JSON 으로 모델링하는 것이 좋다.
> couchdb의 이점인 JSON 데이터에 대해 풍부한 쿼리를 수행하려면 성능을 높이는데 index가 있으면 좋다.

## Json 에서 사용할 수 없는 필드 이름
_deleted
_id
_rev
~version

## shim 에서 제공하는 API
GetState, PutState, GetStateByRange, GetStateByPartialCompositeKey, GetQueryResult

marbles02 패브릭 샘플은 chaincode에서 CouchDB 쿼리를 사용하는 방법을 보여줍니다. 그것은 소유자 ID를 chaincode에 전달하여 매개 변수화 된 쿼리를 보여주는 
queryMarblesByOwner () 함수를 포함합니다. 그런 다음 JSON 쿼리 구문을 사용하여 docType 및 소유자 ID와 일치하는 JSON 문서의 State 데이터를 쿼리합니다.
JSON 쿼리를 효율적으로 만들기 위해서는 CouchDB의 인덱스가 필요하며 정렬 된 모든 JSON 쿼리에 필요합니다. 

## index
JSON 쿼리를 효율적으로 만들기 위해서는 CouchDB의 인덱스가 필요하며 정렬 된 모든 JSON 쿼리에 필요합니다. 
인덱스는 / META-INF / statedb / couchdb / indexes 디렉토리의 chaincode와 함께 패키지화 할 수 있습니다. 
각 색인은 확장자가 * .json 인 자체 텍스트 파일에 정의되어야하며 색인 정의는 CouchDB 색인 JSON 구문에 따라 JSON으로 형식이 지정됩니다. 
예를 들어 위의 대리석 쿼리를 지원하기 위해 docType 및 owner 필드에 대한 샘플 인덱스가 제공됩니다.

## selector
CouchDB selector 문법
[find-selector](http://docs.couchdb.org/en/latest/api/database/find.html#find-selectors)


##secondary index
`map & reduce` 를 통해서 index 기능을 구현
2nd index 를 만들어놓은 다음에 이를 view 와 같은 형태로 저장하고 index 내에서 검색을 한다. 새로운 데이터가 추가되거나 변경이 되더라고 전체 index 를 rebuild 하는 것이 아니라, incremental 방식으로 변경된 부분만 재생성한다. Index 를 rebuild 하는 과정에서 서버가 부하를 받아서 성능이 떨어질 수 있으나 초당 수천건 정도의 index 변경은 수용할 수 있는 수준이다.

## Replication
CouchDB의 강력한 기능중의 하나가 복제 기능이다. ?

## Map/Reduce 는 뭔가요?
대용량 데이터 처리를 위한 분산 프로그래밍 모델
흩어져 있는 데이터를 수직화하여, 그 데이터를 각각의 종류별로 모으고 (Map) Filtering 과 Sotring 을 거쳐 데이터를 뽑아내는 (Reduce) 하는 분산처리 기술과 관련 프레임워크를 의미

## http api
https://flyburi.com/481


# ref
[CouchDB as the State Database](https://monthlyblockchain.tistory.com/category/%5B%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%5D%20%ED%95%98%EC%9D%B4%ED%8D%BC%EB%A0%88%EC%A0%80/%5B%ED%95%98%EC%9D%B4%ED%8D%BC%EB%A0%88%EC%A0%80%20%ED%8C%A8%EB%B8%8C%EB%A6%AD%5D?page=5)
[marbles02](https://github.com/IBM/blockchain-application-using-fabric-java-sdk/blob/master/network_resources/chaincode/src/github.com/marbles02/marbles_chaincode.go)
[docs](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_tutorial.html)