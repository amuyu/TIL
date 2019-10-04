
# Using CouchDB
이 글은 fabric document 의 번역 글입니다.
CouchDB 를 Hyperledger Fabric 이 있는 상태 데이터베이스로 사용하는데 필요한 단계를 설명합니다.

1. index 만들기
2. chaincode 폴더에 index 추가
3. 체인코드 설치 및 인스턴스화
4. CouchDB 상태 데이터베이스 쿼리
5. index 업데이트
6. index 삭제

CouchDB 를 사용할 때, docType 속성을 포함시켜 체인 코드 네임 스페이스에 있는 각 문서 유형을 구별하는 것이 좋다.


couchdb 의 view 를 이용해 Indexing, Grouping, Sorting 등 강력한 기능을 사용할 수 있다.


index 를 정의하려면 세가지 정보가 필요하다. - view
- field : 자주 조회되는 필드
- name : index 의 이름
- type : 이 컨테스트에서 항상 json
```json
{
    "index": {
        "fields": ["foo"]
    },
    "name" : "foo-index",
    "type" : "json"
}
```
index 를 정의할 때 인덱스 이름과 함께 ddoc 속성 및 값을 포함시키는 것이 좋다.
필요한 경우 나중에 index 를 갱신하거나 index 를 명시적으로 지정할 수 있다.

couchbase 기준 설정 내용
하나의 버킷당 4개의 Design Document 정도가 적절하며, 하나의 Design Document당 최대 10개 정도의 뷰를 구현하는 것이 적절하다고 권고하고 있다
(버킷당 40개 정도의 뷰)
여기서 말하는 뷰가 index 의 일종 
버킷은? database 와 유사한 개념?
Design Document? 뷰의 묶음 단위를 말한다.
카우치베이스에는 table 개념이 없기 때문에 document 에 type 이라는 속성을 추가해서 유사한 데이터를 묶어서 관리해줘야 한다.
체인코드의 경우의 체인코드당 하나의 bucket 으로 보면 될 듯하다. 

## chaincode folder 에 index 를 추가하는 방법
sdk 를 사용하여 체인코드를 설치하는 동안 metadataPath 를 포함시킬 수 있다.
cli 에서 설치하는 경우 JSON 인덱스 파일은 META-INF/statedb/couchdb/indexs 경로 아래에 있어야 한다.

## query chaincode
```json
"selector": {
  "$title": "Live And Let Die"
},
"fields": [
  "title",
  "cast"
]

{\"selector\":{\"docType\":\"marble\",\"owner\":\"tom\"}, \"use_index\":[\"_design/indexOwnerDoc\", \"indexOwner\"]}"
```

## sort
sort 하기 위해서는 field 가 index 가 걸려 있어야 하는듯?

## compositeKey
CreateCompositeKey
```go
//  ==== Index the marble to enable color-based range queries, e.g. return all blue marbles ====
//  An 'index' is a normal key/value entry in state.
//  The key is a composite key, with the elements that you want to range query on listed first.
//  In our case, the composite key is based on indexName~color~name.
//  This will enable very efficient state range queries based on composite keys matching indexName~color~*
indexName := "color~name"
colorNameIndexKey, err := stub.CreateCompositeKey(indexName, []string{marble.Color, marble.Name}
```

GetStateByPartialCompositeKey
```go
// Query the color~name index by color
// This will execute a key range query on all keys starting with 'color'
coloredMarbleResultsIterator, err := stub.GetStateByPartialCompositeKey("color~name", []string{color})

// Iterate through result set and for each marble found, transfer to newOwner
var i int
for i = 0; coloredMarbleResultsIterator.HasNext(); i++ {
  // Note that we don't get the value (2nd return variable), we'll just get the marble name from the composite key
  responseRange, err := coloredMarbleResultsIterator.Next()

  // get the color and name from color~name composite key
  objectType, compositeKeyParts, err := stub.SplitCompositeKey(responseRange.Key)
  returnedColor := compositeKeyParts[0]
  returnedMarbleName := compositeKeyParts[1
}
```




# cmd
GetState, PutState, GetStateByRange, GetStateByPartialCompositeKey, 
GetQueryResultWithPagination
GetQueryResult
GetHistoryForKey

## index
CreateCompositeKey

## paging
bookmark 부터 pageSize 까지?
```go
//peer chaincode query -C myc1 -n marbles -c '{"Args":["queryMarblesWithPagination","{\"selector\":{\"owner\":\"tom\"}}","3",""]}'
resultsIterator, responseMetadata, err := stub.GetQueryResultWithPagination(queryString, pageSize, bookmark)
```
boomark 를 사용하기 위해서는 Response 에 bookmark 를 포함해서 던져줘야 하고 들고 있어야 한다.
[{"ResponseMetadata":{"RecordsCount":"0",
"Bookmark":"g1AAAABLeJzLYWBgYMpgSmHgKy5JLCrJTq2MT8lPzkzJBYqz5yYWJeWkmoKkOWDSOSANIFk2iCyIyVySn5uVBQAGYhR1"}}]

# 참고
[Using CouchDB](https://miiingo.tistory.com/160)
[couchdb-best-practices](https://github.com/jo/couchdb-best-practices)
[guide-view](https://guide.couchdb.org/draft/views.html)
[뷰(View) 이해하기](https://bcho.tistory.com/928)
[find-selectors](http://docs.couchdb.org/en/latest/api/database/find.html#find-selectors)