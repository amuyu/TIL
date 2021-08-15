# n+1 문제
두 개의 엔티티가 서로 연관관계에 있을 때, (`OneToMany`, `ManyToOne`) N+1 문제가 발생한다.

## 패치조인
가장 일반적인 방법은 SQL fetch join 조인을 사용하는 것이다.
```
SELECT m FROM Member m JOIN FETCH m.orders
```

JPQL 의 성능 튜닝을 위해 제공되는 조인으로 연관된 엔티티 or 컬렉션을 SQL 한번에 조회하는 기능이다. (기존 SQL 의 조인이 아님)
List -> Set 으로 변경한다.




## 하이버네이트 @BatchSize
BatchSize 어노테이션을 이용하면 연관된 엔티티를 조회할 때, 지정된 size 만큼 SQL 의 IN 절을 사용해서 조회한다.
```
// class
@Entity
class Member{
    @Id @GeneratedValue
    private Long id;

    @org.hibernate.annotations.BatchSize(size = 5)
    @OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}
// sql
SELECT * FROM
ORDER_ WHERE MEMBER_ID IN(
    ?, ?, ?, ?, ?
)
```

## 하이버네이트 @Fetch(FetchMode.SUBSELECT)
연관된 데이터를 조회할 때 서브쿼리를 사용해서 N+1 문제를 해결한다.
```
@Entity
class Member{
    @Id @GeneratedValue
    private Long id;

    @org.hibernate.annotations.Fetch(FetchMode.SUBSELECT)
    @OneToMany(mappedBy = "member", fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}

SELECT * FROM Member;
SELECT * FROM Order_
    WHERE MEMBER_ID IN(
        SELECT ID
        FROM Member
    )
```
즉시로딩으로 설정하면 조회시점에, 지연로딩으로 설정하면 지연로딩된 엔티티를 사용하는 시점에 위의 쿼리가 실행된다.

# queryDsl



# 참고
[JPA 성능 최적화](https://joont92.github.io/jpa/JPA-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/)
[N+1 쿼리 문제와 해결](https://meetup.toast.com/posts/87)
[N+1 문제 및 해결방안](https://jojoldu.tistory.com/165)