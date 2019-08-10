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

```go
func main() {
	req, err := http.NewRequest("GET", "http://csharp.tips/feed/rss", nil)
	if err != nil {
		panic(err)
	}

	req.Header.Add("User-Agent", "Crawler")

	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	bytes, _ := ioutil.ReadAll(resp.Body)
	str := string(bytes)
	fmt.Println(str)
}
```

http.NewRequest 를 호출해서 request 객체를 생성한다.
NewRequest 는 method, url, body 를 입력파라미터로 받을 수 있다.
생성한 request 객체에 req.Header.Add 명령으로 header 를 추가할 수 있다.
http.Client 객체를 생성한 후에 Do 명령으로 요청하고 응답을 받을 수 있다.


# HTTP POST 호출
http.Post() 메서드는 웹서버로 간단히 데이터를 POST 하는데 사용한다.

```go
import (
	"bytes"
	"io/ioutil"
	"net/http"
)

func main() {
	// string to buffer
	reqBody := bytes.NewBufferString("Post plain text")
	resp, err := http.Post("http://httpbin.org/post", "text/plain", reqBody)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	respBody, err := ioutil.ReadAll(resp.Body)
	if err == nil {
		str := string(respBody)
		println(str)
	}
}
```
Post 는 url, contentType, body 를 파라미터로 받는다. 예제에서는
bytes 를 사용해 string 을 buffer 로 변환해서 body로 넘겨주고 있다.
request 후, response 는 GET 호출 시와 동일하게 처리하면 된다.

string 을 buffer 로 변환하는 코드
```go
reqBody := bytes.NewBufferString("Post plain text")
```

## HTTP PostForm 호출
http.PostForm() 은 Form 데이터를 보내는데 유용한 메서드이다.
아래 예제는 Name과 Age 데이터를 Form 형식으로 웹서버에 전송하는 예이다.
```go
import (
	"io/ioutil"
	"net/http"
	"net/url"
)

func main() {
	resp, err := http.PostForm("http://httpbin.org/post", url.Values{"Name": {"Lee"}, "Age": {"10"}})
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	respBody, err := ioutil.ReadAll(resp.Body)
	if err == nil {
		str := string(respBody)
		println(str)
	}
}
```

http.PostForm 은 url과 data 를 param 으로 받는다.
data 는 url.Values type 의 객체를 넣어주면 된다.

url.Values 정의는 다음과 같다.
Named Type 으로 Values 는 map[string][]string 에 이름을 붙인 타입이다.
```go
type Values map[string][]string
```

## JSON 데이타 POST
위 Text POST 와 비슷하다. 다만 데이타가 JSON 포맷이므로 http.Post() 의 두번째 파라미터에 application/json 을 적고,
세번째 파라미터에 JSON 으로 인코딩된 데이타를 전달하면 된다. 
```go
import (
	"bytes"
    "encoding/json"
    "io/ioutil"
    "net/http"
)

type Person struct {
	Name string
	Age int
}

func main() {
	person := Person{"Alex", 10}
	pbytes, _ := json.Marshal(person)
	buff := bytes.NewBuffer(pbytes)
	resp, err := http.Post("http://httpbin.org/post", "application/json", buff)

	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	respBody, err := ioutil.ReadAll(resp.Body)
	if err == nil {
		str := string(respBody)
		println(str)
	}
}
```
json 변환을 위해서 `encoding/json` 패키지를 사용하면된다.
생성한 Person 객체를 json.Marshal 메소드를 호출해서 []byte 슬라이스를 리턴받은 후,
bytes 패키지를 사용해 buffer 객체로 변환해서 Post 함수 호출 시, 사용한다.
response 처리는 전과 동일하다.

http.Post 를 사용하지 않고 보다 세밀한 제어를 위해 Request 객체를 만들어 호출할 수도 있다.



# ref
[GO 활용](http://golang.site/Go/Applications)