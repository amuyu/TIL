
# 버전 관리
git tag 를 사용해서 버전을 관리한다.

2.x 가 될때 까지는 tag 를 따서 배포한다.

2.x 가 넘어가는 경우에는 v2 라는 이름으로 새로운 브랜치를 생성한 후에
go module path 에 /v2 와 같이 새로운 버전을 표기하고 태그로 배포한다.

# 샘플
## 패키지 초기화
```bash
$ go mod init github.com/breezymind/hello
go: creating new go.mod: module github.com/breezymind/hello
```

## 0.1.0 버전 릴리즈
```bash
/* 개발이 완료 된 이후에 첫 릴리즈 버전을 v0.1.0 이라고 가정하고 배포 */
/* 로컬 태그 생성과 메시지 */
$ git tag -a v0.1.0 -m'v0.1.0 release~!!'

/* 로컬 태그 삭제는 git tag -d v0.1.0 */

/* 태그 목록 확인 */
$ git tag -l
v0.1.0

/* 원격 서버에 태그 생성 */
$ git push origin v0.1.0
/* 원격 서버에서 태그 삭제는 git push origin :v0.1.0 */
```
import
```go
import github.com/breezymind/hello
```

## v2.x 릴리즈
v2.x 이상은 브랜치를 새로 생성하여 관리하도록 한다
```sh
$ git checkout -b v2
```
go.mod 상단을 아래와 같이 끝에 브랜치 버전 수정
```
module github.com/breezymind/hello/v2
```
tag 붙여서 push
```bash
$ git add .
$ git commit -m'v2 브랜치 추가'

/* 원격 서버에 v2 브랜치 추가 */
$ git push origin v2
/* 원격 브랜치 삭제는 git push origin :v2 */

$ git tag -a v2.0.0 -m'v2.0.0 release~!!'
$ git push origin v2.0.0
```
import
```go
import github.com/breezymind/hello/v2
```

# ref