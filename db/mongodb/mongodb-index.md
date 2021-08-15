
# Basic
- MongoDB 는 컬렉션당 최대 64개의 인덱스만 생성 가능
- 너무 많은 인덱스를 추가하면 side effects 가 발생

# Index prefix 를 사용
Index prefixes are the beginning subsets of index fields.
```
db.nbaGame.createIndex({teamCode: 1, season: 1, gameDate: 1})
```
Index prefixes of the index, index 의 순서대로 
- {teamCode:1, season: 1, gameDate: 1}
- {teamCode:1, season: 1}
- {teamCode:1}

# 멀티 소팅
Sort key 들은 반드시 인덱스와 같은 순서로 나열되어야만 한다.
signle-field 인데스의 경우, 소팅 방향을 걱정할 필요 없음
compund 인덱스의 경우 소팅 방향이 중요함. index key pattern 또는 index key pattern 의 inverse 와 일치해야함 (동일한 소팅 방향)
```
db.nbaGame.createIndex({gameDate:1, teamName:1});
db.nbaGame.createIndex({gameDate:-1, teamName:1});
db.nbaGame.createIndex({teamName:1, gameDate:1});
db.nbaGame.createIndex({teamName:-1, gameDate:1});
```

# 속도 올리기
- 하나의 컬렉션을 여러 컬렉션으로 나누자
- 여러 개의 thread 에서 bulk operation 으로 많은 document 를 한번에 write
- mongodb 4.0 으로 업그레이드

# 미운 index
hint 를 이용해 강제로 인덱스를 태우기
```
db.consert.count({
    serviceCode: { $in: [2,3]},
    startDate: { $lte: 20190722},
    endDate: { $gte: 20190722},
    {hint: "serviceCode_1_startDate_1_end_e_1 }
});
```


# ref
[Naver 속도의 속도에의한 속도를 위한 몽고 DB](https://www.slideshare.net/mongodb/naver-db-db-naver) - 2019.9.16
[index annotation](https://www.baeldung.com/spring-data-mongodb-index-annotations-converter)