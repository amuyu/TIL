
## db 접속
```
psql
psql -U postgres
psql -h 10.52.0.1
psql -h 10.52.0.1 -p 9000 -U name dbName
```

## db instance 변경
```
\c {db_name} {user_name}
```

## db 목록 조회
```
\list
```

## table 목록 조회
```
# table list
\dt
# sequence list
\ds
# function list
\df
# user list 
\du
# Table 상세 조회
\d {table_name}
# command history
\g
\s
```

# 사용자 생성
* SUPERUSER | NOSUPERUSER ; Superuser 여부. 기본값은 NOSUPERUSER이다.
* CREATEDB | NOCREATEDB ; DB생성 권한 부여 여부. 기본값은 권한 없음 이다.
* CREATEUSER | NOCREATEUSER ; User생성 권한 부여 여부. 기본값은 권한 없음 이다.
* PASSWORD 'password' ; Password 설정
```
create user {userName} password {password} {option};
create user alice password alice superuser;
ALTER ROLE {userName} CREATEDB REPLICATION;
```

# database 생성
```
create database {dbName} owner {userName}
alter database {dbName} owner to {userName}

```



[알아두면 유용한 psql 명령어 정리](https://browndwarf.tistory.com/51)
