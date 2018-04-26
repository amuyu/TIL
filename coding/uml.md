
# has-a
- assocation : 연관 (ref 유지)
- composition : 완전한 소유, 생성과 소멸을 같이 한다.(life cycle이 같다.)
- aggregation : 집합, 생성과 소멸이 같지 않다.  (life cycle이 다르다.)
- dependency : 의존 (ref 유지 안함)
# is-a
- generalization : 일반화, 상속
- realization : 전문화, 인터페이스 구현

# plantUml
@startuml
Alice -> Bob: test
@endum

## 참고
[UML 기호 정리](http://webie.tistory.com/149)
[UML 기본편](http://geniusduck.tistory.com/entry/UML-기본편-기본-표기-형식-및-관계표현법)
[UML 기호 정리](http://fumin.tistory.com/45)
[plantuml](http://plantuml.com/starting)
