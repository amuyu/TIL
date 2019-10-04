# Go modules 시작하기
Go modules 는 vgo, dep, glide 등으로 파생되었던 버전관리의 표준을 이야기하며 go path의 종말을 외쳤습니다.
패키지간 종속성 관리를 golang team 이 표준화하여 관리하는 정책입니다. (Russ Cox)

Golang 으로된 프로젝트를 동시에 여러개 진행(페이스북 웹훅 클러스터) 하면서 불편했던 것 중 하나가
GOPATH 설정 바꾸기였다.

# 따라하기
프로젝트 생성
```bash
go mod init github.com/aidanbae/hello
```

해당 명령어를 실행하면 go.mod 라는 파일이 생성된다.
```bash
module github.com/aidanbae/hello
```
메인 함수를 작성한다.
```go
package main

import (
   "fmt"
   "rsc.io/quote"
)
func main() {
   fmt.Println(quote.Hello())
}
```
이제 라이브러리를 받는다.
```bash
go build
```
go build, go test 같은걸 하면 자동으로 go.mod가 업데이트되고 필요하면 다운로드 받는다. 
라이브러리를 받은 후, go.mod 파일을 읽어본다.
```bash
$ cat go.mod

module github.com/aidanbae/hello

require rsc.io/quote v1.5.2
```

`go list -m all` : 최종 버전의 직간접적인 디펜더시 리스트를 보여준다.
`go get <module-path>@<module-query> : 버전을 지정해 모듈을 추가한다.
`go mod tidy` : 불필요한 종속성을 제거한다.
`go mod vendor` : vendor/ 디렉토리를 만드는 옵션 커맨드
`go mod verify` : 로컬에 설치된 모듈의 해시값과 go.sum을 비교해서 모듈의 유효성을 검증한다.

# 모듈
모듈은 단일 유닛으로 버전 패키지의 집합체이다.
모듈은 정확한 종속성 요구사항을 기록하고 실행 가능한 빌드를 만드는 역할을 가진다.

semantic verion : v(major).(minor).(patch) 를 따릅니다.

# ref
[Go Modules 살펴보기](https://velog.io/@kimmachinegun/Go-Go-Modules-살펴보기-7cjn4soifk)
[Go Modules 시작하기](https://aidanbae.github.io/code/golang/modules/)
[using go modules](https://blog.golang.org/using-go-modules)