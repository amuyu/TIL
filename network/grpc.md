# grpc-go quick start
https://github.com/grpc/grpc-go/tree/master/examples
`helloworld/helloworld.proto` 를 사용해서 client 와 server 간 통신함

## INSTALL
```sh
$ go get -u google.golang.org/grpc/examples/helloworld/greeter_client
$ go get -u google.golang.org/grpc/examples/helloworld/greeter_server
```

## TRY IT!
run the server : greeter_server &
run the client : greeter_clinet
```sh
// result
2019/07/26 16:10:51 Received: world
2019/07/26 16:10:51 Greeting: Hello world
```


# 예제 코드 살펴보기(go)
## helloworld.proto
HelloRequest 와 HelloReply message 를 정의하고
Greeter Service 에 SayHello method 를 정의한다.
## server
`net`, `grpc` 패키지 사용
net.listen 으로 server 생성, tcp 프로토콜과 port 설정
grpc.NewServer() 로 grpc 서버를 생성하고 pb.RegisterGreeterServer() 파일에 넣어준다.
(proto buffer 코드에 grpcserver 에 필요한 코드들이 구현되어 있다)
s.Serve 로 listener 로 들어오는 connection 을 받아들인다.
## client
`grpc` 패키지 사용
grpc.Dial 로 server 에 연결한다.
pb.NewGreeterClient method 를 호출해 client 객체를 생성하고 SayHello method 를 호출한다.


# json 을 사용한 grpc? 
