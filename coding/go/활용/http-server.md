# http server
net/http 패키지는 웹 관련 서버 (및 클라이언트) 기능을 제공한다.
주요한 패키지 메서드로 ListenAndServe(), Handle(), HandleFunc() 등이 있다.

ListenAndServe() 메서드는 지정된 포트에 웹 서버를 열고 클라이언트 Request를 받아들여 
새 Go 루틴에 작업을 할당하는 일을 한다. Handler()과 HandleFunc() 는 요청된 Request Path 에 
어떤 Request 핸들러를 사용할 지를 지정하는 라우팅 역할을 한다.

# HandleFunc
아래 예제는 간단한 HTTP 서버를 구현한 예로 브라우저에서 http://localhost:5000/hello 라고 치면, 
HandleFunc() 의 /hello Path 에 대한 익명함수를 실행하여 Hello World 를 출력하게 된다.

// HandleFunc registers the handler function for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.

HandleFunc 는 DefaultServeMux 에 function 을 등록한다한
http.ResponseWriter 파라미터는 HTTP Response 에 무언가를 쓸 수 있게 하며,
http.Request 파라미터는 입력된 Request 요청을 검토할 수 있게 한다.

ListenAndServe 메서드는 2개의 파라미터를 갖고 있는데, 첫번째는 포트 5000에서
Request 를 Listen 할 것을 지정하고, 두번째는 어떤 ServeMux를 사용할 지를 지정한다. Nil 인 경우 DefaultServeMux를 사용한다.

```go
func main() {
    http.HandleFunc("/hello", func(w http.ResponseWriter, req *http.Request) {
        w.Write([]byte("Hello World"))
    })
 
    http.ListenAndServe(":5000", nil)
}
```


# Handle
http.Handle() 메서드를 사용해서 HTTP Handler 를 정의할 수 있다.
http.Handle 메서드는 첫번째 파라미터로 URL 을 받아들이고, 두번째 파라미터로
http.Handler 인터페이스를 갖는 객체를 받아들인다.
```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

아래 예제는 http.Handler 인터페이스를 갖는 testHandler 라는 struct 를 정의하고
이 struct의 메서드 ServeHTTP() 를 구현한 예이다.
```go

func main() {
	http.Handle("/", new(testHandler))
	http.ListenAndServe(":5000", nil)
}

type testHandler struct {
}

func (h *testHandler) ServeHTTP(w http.ResponseWriter, req *http.Request)  {
	str := "Your Request Path is " + req.URL.Path
    w.Write([]byte(str))
}
```


# 간단한 Static 파일 핸들러 
HTML, 이미지, JavaScript 파일 등의 정적(Static) 파일이 요청되었을 때, 
웹 서버에서 해당 Static 파일들을 적절한 헤더와 함께 전달하는 간단한 핸들러를
구현할 수 있다.

아래 예제는 staticHandler 라는 새 타입을 만들고 이 타입의 ServeHTTP 메서드를
구현한 예제이다.  Request URL 패스에 표시된 정적 파일을 서버 상의 특정 폴더
(wwwroot) 에서 읽어 들여  그 파일 내용을 그대로 전달하고 있다.

```go
func main() {
	http.Handle("/", new(staticHandler))
	http.ListenAndServe(":5000", nil)
}

type staticHandler struct {
}

func (h *staticHandler) ServeHTTP(w http.ResponseWriter, req *http.Request)  {
	path := "wwwroot/" + req.URL.Path
	content, err := ioutil.ReadFile(path)
	if err != nil {
		w.WriteHeader(404)
        w.Write([]byte(http.StatusText(404)))
        return
	}
	w.Header().Add("Content-Type", contentType)
	w.Write(content)
}
```

# http.FileServer 를 사용한 간단한 핸들러
특정 폴더 안에 있는 정적 파일들을 웹서버에서 클라이언트로 그대로 전달하기 위해
내장 기능인 http.FileServer() 를 사용할 수 있다. http.FileServer 는 해당 디렉토리 내의 모든 정적 리소스를 1대1로 매핑하여 그대로 전달한다.

```go
func main() {
	http.Handle("/", http.FileServer(http.Dir("wwwroot")))
	http.ListenAndServe(":5000", nil)
}
```

/ path 로 접속하면 wwwroot 폴더 안의 파일들을 목록 형태로 볼 수 있고 파일을 선택하면 해당 파일의 내용에 접근할 수 있다.

시작 위치를 static 으로 변경하려면 Handle 메소드의 첫번째 파라미터를
/static 으로 변경하면 된다고 하는데 설정해보면 되진 않는다.. 왜?

# oreo

http 패키지를 사용해서 간단한 server 를 구현할 수 있다.

http 패키지는 서버를 구현하기 위한 다음 메소드들을 제공한다.
ListenAndServe(), Handle(), HandleFunc() 

다음 메소드를 사용해서 간단한 예제를 만들수 있다.

간단한 http server 를 만들 때는 http 패키지를 사용하면 된다.