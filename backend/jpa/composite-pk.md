
# JPA entity composite pk 
JPA 에서 composite key 를 표현하기 위해서 PK class 를 정의해야 한다.
PK class 는 여러 조건을 만족해야 하는데 그 중에서 `Serializable` 구현이 필수이다.

1. PK Class는 public이어야하고 public no-arg 생성자.
2. property-based 접근이 사용될 경우, 해당 properties도 public or protected.
3. implements <Serializable>
4. equals 및 hashCode 메소드를 정의, 구현 해야한다.  해당 메소드의 결과는 Database의 동일성 Check 결과와 같아야 한다.
5. 복합 기본 키는 (@EmbeddedId, @IdClass) 어노테이션으로 표현한다.

예를 들어 다음과 같은 형태이다.
```java
package com.xx.entity;
 
import java.io.Serializable;
 
/**
 * The primary key class for the xx database table.
 */
//@Embeddable = EmbeddedId 경우 필요
public class xxPK implements Serializable {
	//default serial version id, required for serializable classes.
	private static final long serialVersionUID = 1L;
 
	private String createdate;
	private int cultcode;
 
	public xxPK() {
	}
	public String getCreatedate() {
		return this.createdate;
	}
	public void setCreatedate(String createdate) {
		this.createdate = createdate;
	}
	public int getCultcode() {
		return this.cultcode;
	}
	public void setCultcode(int cultcode) {
		this.cultcode = cultcode;
	}
 
	public boolean equals(Object other) {
		...
	}
 
	public int hashCode() {
		...
	}
}
```


http://blog.breakingthat.com/2018/03/16/jpa-entity-%EB%B3%B5%ED%95%A9pk-%EB%A7%B5%ED%95%91-embeddedid-idclass/