
# Database 생성
use DATABASE_NAME 명령어를 통하여 Database 를 생성할 수 있습니다.
데이터베이스가 이미 존재하는 경우엔 해당 데이터베이스를 사용합니다.
```
> use mongodb_tutorial
```
현재 사용중인 데이터베이스를 확인하려면 `db` 명령을 입력합니다.
```
> db
mongodb_tutorial
```
내가 만든 데이터베이스의 리스트를 확인하려면 show dbs 명령을 입력합니다.
```
> show dbs
local             0.000GB
mongodb_tutorial  0.000GB
```

# Database 제거
Database 를 제거할 때는 db.dropDatabase() 명령을 사용합니다.
```
> use mongodb_tutorial
> db.dropDatabase()
{ "dropped" : "mongodb_tutorial", "ok" : 1 }
```

# Collection 생성
Collection 을 생성할 때는 db.createCollection(name, [options]) 명령어를 사용합니다.
name 은 설정하려는 컬렉션의 이름이며 option 은 document 타입으로 구성된 해당 컬렉션의 설정값입니다.
```
> use test
> db.createCollection("account")
{ "ok" : 1 }
> db.createCollection("account", {
... capped: true,
... autoIndex: true,
... size: 6142800,
... max: 10000
... })
{ "ok" : 1 }
> db.account.insert({"name": "amuyu"})
WriteResult({ "nInserted" : 1 })
```

# Collection 제거
Collection 을 제거할 때는 drop() 명령을 사용합니다.
```
> use test
> db.account.drop()
true
> show collections
```

# Document 추가
insert() 메소드를 사용하여 Document 를 추가할 수 있습니다.
명령어를 사용하기 전 데이터를 추가 할 데이터베이스를 선택해주어야 합니다.
```
> use test
> db.account.insert({"name": "amuyu"})
WriteResult({ "nInserted" : 1 })
> db.account.insert([
    {"name": "amuyu1"},
    {"name": "amuyu2"},
])
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
```

# Document 제거
remove(criteria,justOne) 메소드를 사용하여 Document 를 제거할 수 있습니다.
- criteria : 삭제할 데이터의 기준 값
- justOne : 이 값이 true 이면 1개의 document 만 제거합니다.
```
> db.books.remove({"name": "Book1"})
```

------

# Document Query - find() 메소드
find(query, projection) 메소드를 사용하여 Document 를 조회할 수 있습니다.

## parameter
- query(document) : Optional, document 를 조회할 때, 기준을 정합니다. 
- projection(document) : Optional 다큐먼트를 조회할 때 보여질 field 를 정합니다.

## return
조건에 해당하는 Document 들을 선택하여 cursor 를 반환합니다. cursor 는 query 요청의 결과값을 가르키는 pointer 입니다.
cursor 객체를 통하여 보이는 데이터의 수를 제한할 수 있고, 데이터를 sort 할 수도 있습니다.
이는 10분동안 사용되지 않으면 만료됩니다. 

## example
샘플데이터 
```
[
  {
    "title": "article01",
    "content": "content01",
    "writer": "Velopert",
    "likes": 0,
    "comments": []
  },
  {
    "title": "article02",
    "content": "content02",
    "writer": "Alpha",
    "likes": 23,
    "comments": [
      {
        "name": "Bravo",
        "message": "Hey Man!"
      }
    ]
  },
  {
    "title": "article03",
    "content": "content03",
    "writer": "Bravo",
    "likes": 40,
    "comments": [
      {
        "name": "Charlie",
        "message": "Hey Man!"
      },
      {
        "name": "Delta",
        "message": "Hey Man!"
      }
    ]
  }
]
```
*예제1* : 모든 다큐먼트 조회
```
> db.articles.find()
```
*예제2* : 다큐먼트를 예쁘게 깔끔하게 조회
```
> db.articles.find().pretty()
```
*예제3* : writer 의 값이 "Velopert" 인 Document 조회
```
> db.articles.find({"writer": "Velopert"}).pretty()
```
*예제4* : likes 값이 30이하인 Document 조회
```
> db.articles.find( { “likes”: { $lte: 30 } } ).pretty()
```
*예제5* : likes 값이 10보다 크고 30 보다 작은 Document 조회
```
> db.articles.find( { “likes”: { $gt: 10, $lt: 30 } } ).pretty()
```
*예제6* : writer 값이 ["Alpha", "Bravo"] 안에 속하는 값이 Document 조회
```
> db.articles.find( { “writer”: { $in: [ “Alpha”, “Bravo” ] } } ).pretty()
```
*예제7* : title 값이 "article" 이거나, writer 값이 "Alpha" 인 Document 조회
```
> db.articles.find({ $or: [ { “title”: “article01” }, { “writer”: “Alpha” } ] })
```
*예제8* : writer 값이 "Velopert" 이고 likes 값이 10 미만인 Document 조회
```
> db.articles.find( { $and: [ { “writer”: “Velopert” }, { “likes”: { $lt: 10 } } ] } )
```


## Query 연산자
[mongodb operators](https://docs.mongodb.com/v3.2/reference/operator/query/)

### 비교 연산자
- $eq : 주어진 값과 일치하는 값
- $gt : 주어진 값보다 큰 값
- $gte : 주어진 값보다 크거나 같은 값
- $lt : 주어진 값보다 작은 값
- $lte : 주어진 값보다 작거나 같은 값
- $ne : 주어진 값과 일치하지 않는 값 
- $in : 주어진 배열 안에 속하는 값 
- $nin : 주어진 배열 안에 속하지 않는 값

## $regex 연산자
$regex 연산자를 통하여 Document 를 정규식을 통해 찾을 수 있습니다. 이 연산자는 다음과 같이 사용합니다.
```
{ <field>: { $regex: /pattern/, $options: '<options>' } }
{ <field>: { $regex: 'pattern', $options: '<options>' } }
{ <field>: { $regex: /pattern/<options> } }
{ <field>: /pattern/<options> }
```
options 는 다음과 같습니다.
- i : 대소문자 무시
- m : 정규식에서 anchor(^)를 사용할 때 값에 \n 이 있다면 무력화
- x : 정규식 안에 있는 whitespace 를 모두 무시
- s : dot (.) 사용할 때 \n 을 포함해서 매치

*예제9* : `article0[1-2]` 에 일치하는 값이 title 에 있는 Document 조회
```
db.articles.find( { “title” : /article0[1-2]/ } )
```

## $where 연산자
$where 연산자를 통하여 javascript expression 을 사용할 수 있습니다.
```
> db.articles.find( { $where: “this.comments.length == 0” } )
```

## $elemMatch 연산자
$elemMatch 연산자는 Embedded Documents 배열을 쿼리할 때 사용됩니다. 
mock-up data 에서는 comments 가 Embedded Document 에 속합니다.
```
// Embedded Document 배열
> db.articles.find( { “comments”: { $elemMatch: { “name”: “Charlie” } } } )
// Embedded Document
> db.users.find({ "name.first": "M.J."})
// 그냥 배열
db.users.find({ "language": "korean"})
```

## projection
쿼리의 결과값에서 보여질 field 를 정한다.
*예제12* : article 의 title 과 content 만 조회
```
db.articles.find( { } , { “_id”: false, “title”: true, “content”: true } )
```

### projector - slice 연산자
projector 연산자 중 $slice 연산자는 Embedded Document 배열을 읽을 때 limit 설정을 합니다.
*예제13* : title 값이 article03 인 Document 에서 덧글은 하나만 보이게 출력
```
db.articles.find( { “title”: “article03” }, { comments: { $slice: 1 } } )
```

### projector - elemMatch 연산자
쿼리의 결과값에서 보여질 field 에서 다시, elemMatch 의 항목만 조회한다.
```
db.articles.find(
...     {
...         "comments": {
...             $elemMatch: { "name": "Charlie" }
...         }
...     },
...     {
...         "title": true,
...         "comments": {
...             $elemMatch: { "name": "Charlie" }
...         },
...         "comments.name": true,
...         "comments.message": true
...     }
... )
```

# sort, limit, skip
find() 메소드를 사용하면 cursor 형태의 결과값을 반환하는데, 
이 객체가 가지고 있는 limit, skip, sort 메소드를 사용해서 결과를 제한하거나 나열할 수 있다.

## sort, cursor.sort(Document)
sort 메소드를 사용해 데이터를 정렬합니다. 매개변수로는 어떤 KEY 를 사용하여 정렬할지 알려주는 document 를 전달합니다.
document 의 구조는 다음과 같습니다.
```
{ key: value }
```
key 는 데이터의 field 이름이고, value 는 1로 설정하면 오름차순, -1로 하면 내림차순으로 정렬합니다.
여러 KEY 를 입력할 수 있고 먼저 입력한 KEY 가 우선권을 갖습니다.
```
> db.orders.find().sort( { "_id": 1 } )
```

## limit, cursor.limit(value) 
데이터 갯수를 제한할 때 사용합니다. value 파라미터는 출력할 갯수 값입니다.
```
> db.orders.find().limit(3)
``` 

## skip, cursor.skip(value)
출력할 데이터의 시작부분을 설정할 때 사용합니다. value 값 개수의 데이터를 생략하고 그 다음부터 출력합니다.


# update
update() 메소드를 사용해서 데이터를 수정할 수 있습니다. 이 메소드의 구조는 다음과 같습니다.
```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
특정 field 를 수정할 수도 있고 이미 존재하는 document 를 대체할 수도 있습니다.
- query(document) : 업데이트 할 document 의 criteria 를 정합니다. find() 메소드에서 사용하는 query 와 같습니다.
- update(document) : document 에 적용할 변동사항입니다.
- upsert(boolean) : Optional, default: false 이 값이 true 로 설정되면, 여러개의 document 를 수정합니다.
- writeConcern(document) : Optional, wtimeout 등 document 업데이트 필요한 설정값입니다.

## schema
```
{ name: "Charlie", age: 23, skills: [ "mongodb", "nodejs"] }
```

## 특정 field 업데이트 하기
특정 field 값을 수정할 땐 $set 연산자를 사용합니다.
```
> db.people.update( { name: "Abet" }, { $set: { age: 20 } } )
```

## document 를 replace 하기
_id 는 바뀌지 않고 document 를 변경할 수 있습니다.
```
> db.people.update( { name: "Betty" }, { "name": "Betty 2nd", age: 1 })
```

## 특정 field 제거하기
$unset 연산자를 사용해 특정 field 를 제거합니다.
```
> db.people.update( { name: "David" }, { $unset: { score: 1 } } )
```

## document 가 존재하지 않는다면 새로 추가
upsert 옵션을 설정하여 document 가 존재하지 않으면 새로 추가합니다.
```
> db.people.update( { name: "Elly" }, { name: "Elly", age: 17 }, { upsert: true })
```

## 여러 document 의 특정 field 를 수정
Multi 옵션을 설정하여 여러 document 의 field 를 수정합니다.
```
> db.people.update(
... { age: { $lte: 20 } },
... { $set: { score: 10 } },
... { multi: true }
... )
```

## 배열에 값 추가하기
$push 연산자를 사용해서 배열에 값을 추가합니다.
```
> db.people.update(
... { name: "Charlie" },
... { $push: { skills: "angularjs" } }
... )

> db.people.update(
... { name: "Charlie" },
... { $push: {
...     skills: {
...         $each: [ "c++", "java" ],
...         $sort: 1
...     }
...   }
... }
... )
```

## 배열에서 값 제거하기
$pull 연산자를 사용해서 배열에 값을 제거합니다.
```
> db.people.update(
... { name: "Charlie" },
... { $pull: { skills: "mongodb" } }
... )

> db.people.update(
... { name: "Charlie" },
... { $pull: { skills: { $in: ["angularjs", "java" ] } } }
... )
```

## group
```
db.getCollection('_149dd5_vc').aggregate( [{ $group : { _id : "$issuerDid" }}])
```


# ref 
[MongoDB velopert 강좌](https://velopert.com/479)
[query](https://docs.spring.io/spring-data/mongodb/docs/1.2.0.RELEASE/reference/html/mongo.repositories.html)