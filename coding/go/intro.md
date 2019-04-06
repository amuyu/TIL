# go
구글이 만든 언어
Go언어는 컴파일 언어이다
Go언어로 구현된 컴파일러가 있다
컴파일러의 속도가 매우 빨라 인터프리터 언어처럼 쓸 수 있다

# GoRutine
Go는 GoRoutine 이라는 비동기 메커니즘을 제공한다.
각각의 고루틴은 병렬로 동작하며 메시지 채널을 통해 값을 주고 받는다.
자체적인 스케줄러에 의해 관리되는 경량 스레드이다.

# 환경변수
서로 다른 머신 플랫폼들을 타겟으로 배포해야 할 경우 환경변수(GOOS, GOARCH)
를 설정한 후 컴파일해서 여러 벌의 배포판을 만들어야 한다.

# keyword
```
break default func interface select
case defer go map struct
chan else goto package switch
const fallthrough if range type
continue for import return var
```

# 설치
Go 프로그래밍을 시작하기 위해서 http://golang.org/dl 에서 해당 OS 버전의 Go를 다운로드 하여 설치

## Mac에 Go 설치
http://golang.org/dl 에서 패키지 파일을 다운받아 설치한다.
패키지는 Go를 `/usr/local/go` 에 설치하며, Go 실행경로인 /usr/local/go/bin 을 자동으로 환경변수에 추가한다.

## Go 실행 테스트
간단하게 Go를 실행해보자
Go 설치 경로로 이동해 bin 폴더에 다음의 코드를 작성해보자
```go
package main
fun main() {
	println(“Test”)
}
```
작성 후, go를 실행한다.
```sh
go run <file>
```

## Workspace 폴더
Go 프로그래밍을 위해 일반적으로 Workspace 폴더를 생성하는데, 이 폴더 안에는
3개의 서브디렉토리 (bin ,pkg, src) 를 생성한다.
Workspace 를 생성한 후, GOPATH 환경변수에 경로를 추가한다.
여러 Workspace가 있는 경우, 이들 경로를 계속 추가한다.

## 환경 변수
Go는 2개의 중요한 환경변수(GOROOT와 GOPATH)를 사용한다.
GOROOT : Go가 설치된 디렉토리, Go실행파일은 bin폴더에 Go 표준 패키지들은 pkg폴더에 있다.
GOPATH : 표준 패키지 이외의 3rd Party나 사용자 정의 패키지들을 GOPATH에서 찾는다.

환경변수는 `go env` 를 실행하여 체크할 수 있다.

## 편집기
atom, visual studio code, eclipse


# 참고
[Go](https://namu.wiki/w/Go)
[예제로 배우는 go 프로그래밍](http://golang.site/go/basics)
