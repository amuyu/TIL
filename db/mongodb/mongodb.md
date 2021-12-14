# mongodb

## 장점
- schema-less 
- 각 객체의 구조가 뚜렷하다.
- Deep Query ability (SQL 만큼 강력한 Query 성능을 제공한다.)
- 애플리케이션에서 사용되는 객체를 데이터베이스에 추가 할 때 Conversion/Mapping 이 불필요하다.


## spring 연결하는 방법


## index 걸기


## select query paging 처리


## bulk 
bulk 형태로 업데이트 하기
https://docs.mongodb.com/manual/reference/method/Bulk.find.update/
```java
BulkOperations bulkOperations = mongoTemplate.bulkOps(BulkOperations.BulkMode.UNORDERED, score.getCollectionName());
Query query = new Query();
Update update = Update.update(key, value);
bulkOperations.updateOne(query, update);
...
bulkOperations.execute();
```

# ref
[spring-data-mongodb-tutorial](https://www.baeldung.com/spring-data-mongodb-tutorial)

-----

