# 파일 읽고 쓰기 살펴보기
//샘플 코드
```go
package main

import (
	"os"
)

func main() {
	fi, err := os.Open("./1.txt")
	if err != nil {
		println("# err:"+ err.Error())
		panic(err)
	}
	defer fi.Close()
}
```

file 을 open 할 때, `os.Open()` 을 사용한다.

`os.Open` 가 Reutrn 하는 error 는 error interface 를 사용한 타입으로
Error() 를 호출하면 에러 메시지를 확인할 수 있다.

panic 은 현재 함수를 멈추고 defer 함수를 모두 실행한 후, 즉시 리턴하는 메소드로
프로그램 에러를 발생하고 종료함

defer 는 java 의 finally 처럼 동작하는 메소드

// 샘플 코드
```go
fo, err := os.Create("./2.txt")
if err != nil {
    panic(err)
}
defer fo.Close()

buff := make([]byte, 1024)

for {
    cnt, err := fi.Read(buff)
    if err != nil && err != io.EOF {
        panic(err)
    }

    if cnt == 0 {
        break
    }

    _, err = fo.Write(buff[:cnt])
    if err != nil {
        panic(err)
    }
} 
```

file 을 생성할 때, `os.Create()` 를 사용한다.
file 을 읽은 때, `file.Reade()`, file 에 쓸 때는 `file.Write()`를 사용한다.

:=, = 차이점은? 새로 assign 할 때 `:=` 를 사용한다. 이미 선언한 경우 `=` 를 사용

buff 라는 slice 를 사용하고 있으며 slice 생성을 위해서 `make` 를 사용하고 있다

return 받을 때 '_' 는 사용하지 않겠다는 의미로 봐도 되나.. 사용할 경우 
> cannot use _ as value
에러가 발생한다.

ioutil 패키지를 사용하면 입력 파일이 매우 크지 않은 경우, ReadFile, WriteFile 함수를 이용해
편리하게 파일을 읽고 쓸 수 있따.
```go
package main

import "io/ioutil"

func main() {
	bytes, err := ioutil.ReadFile("./1.txt")
	if err != nil {
		panic(err)
	}
	err = ioutil.WriteFile("./2.txt", bytes, 0644)
	if err != nil {
		panic(err)
	}
}
```
FileMode 는 Unix rwxrwxrwx permissions 를 사용한다.
String 과 byte[] 를 변환하려면, 다음과 같이 한다
```go
s := "Hello, world!"
b := []byte(s)
ss := string(b)
```

# HTTP GET 호출
// 샘플 코드
```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	resp, err := http.Get("http://csharp.news")
	if err != nil {
		panic(err)
	}

	defer resp.Body.Close()

	data, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%s\n", string(data))
}
```
http 패키지는 웹 관련 클라이언트 및 서버 기능을 제공한다. 
http.Get() 메서드는 쉽게 웹 페이지나 웹 데이터를 가져오는데 사용된다.

Get method 는 `Response` 를 리턴하고 Response 의 Body 는 io.ReadCloser type 으로
`ioutil` 패키지를 사용해서 data 를 읽을 수 있다.

http.Get() 은 한 문장으로 HTTP GET을 호출하는 장점이 있지만 Request 시 헤더를 추가하다던가,
Request 스트림을 추가하는 것과 같은 세밀한 컨트롤을 할 수 없는 단점이 있다.
Request 시 보다 많은 컨트롤이 필요하다면, Request 객체를 직접 생성해서 http.Client 객체를 통해 호출하면 된다.






# ref
[GO 활용](http://golang.site/Go/Applications)