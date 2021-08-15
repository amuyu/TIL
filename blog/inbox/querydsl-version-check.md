# querydsl 에서 버전 문자열 비교하기 (feat.inet_aton)
querydsl 에서 `1.0.0` 같은 문자열 크기를 비교하는 방법을 정리합니다.

여기서는 `inet_aton` 이라는 함수를 사용했습니다.
`inet_aton` 은 `.` 으로 구분된 IPv4 network 주소 문자열을 integer 형태로 변환하는 function 입니다. 
https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet-aton

`1.0.0` 문자열도 `.` 으로 구분된 문자열이라서 `inet_aton` 함수로 정수 형태로 변환할 수 있습니다.



```java
StringTemplate columnInetAton = Expressions.stringTemplate("inet_aton({0})", bannerStructureVersion.appVersion);
StringTemplate versionInetAton = Expressions.stringTemplate("inet_aton({0})", version);
return jpaQueryFactory.select(bannerStructureVersion.id)
	.from(bannerStructureVersion)
	.where(bannerStructureVersion.id.eq(
		JPAExpressions.select(bannerStructureVersion.id.max())
			.from(bannerStructureVersion)
			.where(columnInetAton.loe(versionInetAton))))
	.fetchOne();
```