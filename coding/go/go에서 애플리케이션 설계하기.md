# 개요
Go를 배울 때 가장 어려웠던 부분은 애플리케이션을 어떻게 설계하는가였다.
Go는 그 어떤 프로젝트 구조나 애플리케이션 설계방식을 규정짓고 있지 않으며 Go의 컨벤션은 대개 제각각이다.
Go 애플리케이션의 아키텍쳐를 구성하면서 발견한 정말 많은 도움이 되었던 4가지 패턴을 여러분에게 알려주려고 한다.

## 이 책의 내용은?
Go 애플리케이션 아키텍처를 구성하는데 도움이 된 4가지 패턴 소개

---------------

# 1. 전역 변수를 사용하지 말라
함수 핸들러를 사용하게 되면, 애플리케이션 상태에 접근하는 유일한 방법은 전역 변수를 사용하는 것이다. (1)
클로저를 사용한 핸들러 래핑으로 이를 간단히 할 수 있다. (2)

## (1) 무슨 말인가?
이 말을 하기 전 예를 든 코드를 보자
```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    http.HandleFunc("/hello", hello)
    http.ListenAndServe(":8080", nil)
}

func hello(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "hi!")
}
```
HandleFunc 는 함수 핸들러를 사용하는 함수로 hello 라는 함수를 받아 사용한다.
hello 라는 함수가 호출되어 어떤 기능을 수행하는데 필요한 데이터들을 전역 데이터로 추가해야 할 수 있다.
hello 는 어디에 속해있지 않으므로...... 그래서 제안하는 더 나은 방법은 다음과 같은 코드다

```go
type HelloHandler struct {
    db *sql.DB
}

func (h *HelloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    var name string
    
    // 쿼리 실행.
    row := h.db.QueryRow("SELECT myname FROM mytable")
    if err := row.Scan(&name); err != nil {
        http.Error(w, err.Error(), 500)
        return
    }
    
    // 클라이언트에 전송.
    fmt.Fprintf(w, "hi %s\n", name)
}

...

func main() {
    // 데이터베이스 커넥션을 연다.
    db, err := sql.Open("postgres", "...")
    if err != nil {
        log.Fatal(err)
    }
    
    // 핸들러 등록.
    http.Handle("/hello", &HelloHandler{db: db}
    http.ListenAndServe(":8080", nil)
}
```
Handle 함수는 Handler interface 를 파라미터로 받는다.
HelloHandler 라는 Handler interface 를 구현한 struct 를 만드는데, 이 때, struct 는 db 를 멤버로 들고 있어 
호출 시, db 를 사용해서 데이터를 조회하고 클라인트에 전송하도록 구현되어 있다.
정리하면 HandleFunc 를 사용해 함수 핸들러를 사용하면 함수에서 db 접근이 필요한 경우 db 가 전역 변수로 빠져있거나
연결할 수 있는 다른 메소드가 필요하게 된다. 는 내용으로 보인다.

## (2) 무슨 말인가? 
handler 를 사용할 때, 클로저 형태로 구현하면 위에서 설명하고 있는 interface 를 구현한 HelloHandler 와 동일한 효과를 얻을 수 있다.
```
func helloHandler(db *sql.DB) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    		var name string
    		// Execute the query.
    		row := db.QueryRow("SELECT myname FROM mytable")
    		if err := row.Scan(&name); err != nil {
        		http.Error(w, err.Error(), 500)
        		return
    		}
    		// Write it back to the client.
    		fmt.Fprintf(w, "hi %s!\n", name)
    	})
}
```

---------------

# 2. 애플리케이션에서 바이너리를 분리하라
나는 누군가 "go get'을 실행하면 내 애플리케이션이 자동으로 설치될 수 있도록 main.go 파일을 프로젝트 루트에 넣어 사용한다.
main.go 파일과 애플리케이션 로직을 같은 패키지로 합치게 되면 다음의 두 가지 결과를 갖게 된다.

1. 애플리케이션을 라이브러리로써 재사용할 수가 없다. (1)
2. 오직 하나의 애플리케이션 바이너리만을 가질 수 있다. (2)

이 문제를 해결하기 위해 발견한 가장 좋은 방법은 단순히 프로젝트 내에서 각 서브 디렉토리가 하나의 애플리케이션
바이너리인 "cmd" 디렉토리를 사용하는 것이다.

camilstore 가 설치될 때 빌드되는 4개의 분리된 애플리케이션 바이너리
```
camlistore/
    cmd/
        camget/
            main.go
        cammount/
            main.go
        camput/
            main.go
        camtool/
            main.go
```

## (1) 무슨 말인가?
뭔 말이지
main 패키지와 애플리케이션 로직이 같은 패키지 상에 있을 때의 문제 일 듯
main 패키지의 경우 실행되는 바이너리 파일이기 때문에 라이브러리로써 사용하지 못한다는 내용인가 보다.
main 패키지는 라이브러리가 아닌 실행하기 위한 용도로 사용하는 패키지를 의미 

## (2) 무슨 말인가?
Go program 은 `package main` 이 반드시 있어야 한다고 한다.
main package 가 선언되어 있으면 `go install` 했을 때, 실행할 수 있는 바이너리 파일을 만든다고 한다.
https://medium.com/rungo/everything-you-need-to-know-about-packages-in-go-b8bac62b74cc

프로젝트 루트에 main 을 넣어 만들면 main 은 하나만 존재하므로 하나의 바이너리 파일이 생성된다는 말인듯 하다.
cmd 폴더에는 main package 선언이 가능한듯 하고 여러 바이너리 파일을 만들기 위해서는 위에서 제안한대로
cmd 디렉토리에 여러 main package 를 선언해 여러 바이너리 파일을 만들 수 있다는 말인 듯 하다.

## 핵심은
실행하는 바이너리 파일 관련된 구현은 `cmd` 폴더에서

## 라이브러리 주도 개발 
main.go 파일을 루트 밖으로 옮기는 것은 애플리케이션을 라이브러리의 관점에서 구현할 수 있게 해준다.
애플리케이션 바이너리는 단순히 애플리케이션 라이브러리의 클라이어트이다.

---------------


# 3. 애플리케이션별 컨텍스트를 위한 타입 래핑
애플리케이션 수준의 컨텍스트를 제공하기 위해 몇가지 제너릭 타입을 래핑한다.
DB 와 TX 타입 래핑을 예로 설명한다.
```go
package myapp

import (
    "database/sql"
)

type DB struct {
    *sql.DB
}

type Tx struct {
    *sql.Tx
}

....

// Open은 데이터 소스를 위한 DB 레퍼런스를 반환한다.
func Open(dataSourceName string) (*DB, error) {
    db, err := sql.Open("postgres", dataSourceName)
    if err != nil {
        return nil, err
    }
    return &DB{db}, nil
}

// Begin은 새로운 트랜젝션을 반환하기 시작한다.
func (db *DB) Begin() (*Tx, error) {
    tx, err := db.DB.Begin()
    if err != nil {
        reutnr nil, err
    }
    return &Tx{tx}, nil
}
```

기본 데이터 스토어를 추상화 하는 것은 새 데이터베이스로 교환하거나 다수의 데이터베이스의 사용을 쉽게 만들어준다.

## 핵심 내용은?
db 같은 것을 사용할 때 래핑 등을 이용해 추상화된 형태로 제공하면 사용자는 해당 db 를 직접 컨트롤할 필요가 없어 쉽게 사용할 수 있다.
애플리케이션의 DB & Tx 타입을 호출하는 코드는 모두 감춰져 있다.


-----

# 4. 서브 패키지로 골머리를 앓지 말라
Go는 패키지를 위한 요구조건이 있는데, 순환 의존(cyclic dependencis)을 가질 수 없다는 것이다.
이런한 조건을 만족시키는 상태로 서브패키지를 만들어 사용하면 점점 관리하기가 힘들어진다.

최근 나는 오직 하나의 루트패키지를 사용하는 방법을 택했다. [Bolt](https://github.com/boltdb/bolt) 처럼

큰 패키지를 만드는데 도움이 되는 몇 가지 팁이 있다.
1. 각 파일에 관련있는 타입과 코드를 함께 그룹핑하라. 타입과 함수들이 잘 구성되어 있다면 그 파일은 200에서 500줄의 코드를 가지는 경향이 있다
2. 가장 중요한 타입은 파일의 맨 위에 놓고 밑으로 갈수록 중요성이 낮아지는 순서대로 타입을 추가하라.
3. 애플리케이션이 10,000 라인을 넘어가기 시작하면 이 프로젝트를 더 작은 프로젝트들로 나눌 순 없는지를 심각하게 고민하라.

## 핵심 내용은?
서브 패키지로 세분화 하기 보다는 하나의 루트 패키지를 갖는게 더 좋을 수 있다.


-----

# 결론
코드 구성은 소프트웨어 개발에 있어 가장 어려운 주제중 하나이며 이것이 가져오는 가치에 초점을 맞추는 일은 드물다. 

전역 변수를 적게 사용하고, 
애플리케이션 바이너리 코드를 패키지로 옮기고, 
애플리케이션별 컨텍스트를 위해 타입을 래핑하고, 
서브패키지는 제한하라.
이들은 단지 Go 코드를 쉽게 작성하고 더 나은 유지보수가 가능하도록 도와주는 몇가지 트릭들이다.

Go 프로젝트를 Ruby, Java 또는 Node.js 프로젝트와 같은 방식으로 작성하게되면 아마 언어와 싸우게 될 것이다.



# ref
[Go에서 애플리케이션 설계하기](https://mingrammer.com/translation-structuring-applications-in-go/)