# Spring Data JPA - LazyInitialization 에러
table join 형태의 entity 를 설정해서 조회하는 경우, 발생할 수 있는 에러
```
LazyInitializationException: could not initialize proxy - no session
```

가장 쉬운 해결 방법은 메소드에 `@Transactional`, `@Transactional(readOnly=True)` 를 붙여준다.


## 참고
[Spring Data JPA - LazyInitialization 에러](https://github.com/HomoEfficio/dev-tips/blob/master/Spring%20Data%20JPA%20-%20LazyInitialization%20%EC%97%90%EB%9F%AC%20-%20getOne().md)