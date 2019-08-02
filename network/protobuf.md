# protobuf
구글에서 개발하고 오픈소스로 공개한 직렬화 데이터 구조
다양한 언어를 지원하고 직렬화 속도가 빠르고 직렬화된 파일의 크기가 작다
GRPC 네트워크 프로토콜에서 메세지를 프로토콜 버퍼를 이용하여 직렬화한다.

프로토콜 버퍼를 사용하기 위해서는 저장하기 위한 데이터형을 proto file 이라는 형태로 정의해야 한다.
정의된 데이터 타입을 프로그래밍 언어에서 사용하려면 `protoc` 컴파일러로 해당 언어에 맞는 클래스 파일을 생성한다.

# JSON 변환
프로토콜 버퍼로 저장된 데이터 구조를 JSON 으로 변환하거나 JSON 을 프로토콜 버퍼 객체로 만들 수 있다.
클라이언트에서 JSON 을 프로토콜 버퍼로 직렬화해서 패킷양을 줄여서 전송하고 서버에서는 받은 후에, JSON 으로 풀어서
사용하는 구조가 바로 `GRPC` 구조이다.

# example
[example](https://github.com/protocolbuffers/protobuf/tree/master/examples)
go 나 python 을 사용하면 빠르게 돌려볼수 있을것 같아.. 둘 중에 Go 로 시작

# process
- go plugin 설치
- `make go` 실행 : 예제에 포함된 addressbook.proto 파일로 go 에 맞는 클래스 파일을 생성한다
+ `protoc $$PROTO_PATH --go_out=tutorial addressbook.prot` 
+ `go build -o add_person_go add_person.go`
- add_person.go 파일을 실행해서 data 파일 생성

## protoc install
brew 로 protobuf 설치
```sh
brew install protobuf
```
go protocol buffers plugin 설치
```sh
go get github.com/golang/protobuf/protoc-gen-go
```

## example copy
위에서 받은 example 코드를 `$GOPATH/src/github.com/protocolbuffers/protobuf/examples` 에 복사한다.

## example run
다음 명령을 실행해서 `addressbook.proto` 파일을 go 언어에 맞는 클래스 파일을 생성한다.
```sh
make go
```
tutorial 폴더에 `addressbook.pb.go` 라는 파일이 생성된다.
정의된 내용에 맞게 data 를 생성할 수 있다.
아래 명령을 실행하면 addressbook 정의된 내용을 입력할 수 있고, `addressbook.data` 라는 파일로 생성한다.
```sh
./add_person_go addressbook.data
```
생성한 `addressbook.data` 는 `list_person_go` 로 조회할 수 있다.
```sh
./list_person_go addressbook.data
```

# basic type
bool, int32, float, double, string, enum, repeated

# message
간단한 search request message 를 정의해보자.
```proto3
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

# service
RPC 로 message를 주고받을 때, RPC service 를 정의할 수 있다.
예를 들어 SearchRequest 를 받아서 SearchResponse 를 리턴한다고 하면 다음과 같이 정의할 수 있다.
```proto3
service SearchService {
    rpc Search (SearchRequest) returns (SearchResponse);
}
```


# Field Number(tag)
1-15 : one byte
16 <= : re-encoding

# Protocol Buffer API (Go)
## Writing a Message
`proto` library `Marshal` function 을 사용해서 protocol buffer data로 serialize 할 수 있다.
```go
book := &pb.AddressBook{}
// ...

// Write the new address book back to disk.
out, err := proto.Marshal(book)
if err != nil {
        log.Fatalln("Failed to encode address book:", err)
}
if err := ioutil.WriteFile(fname, out, 0644); err != nil {
        log.Fatalln("Failed to write address book:", err)
}
```
## Reading a Message
`proto` library `Unmarshal` function 을 사용해서 message 를 parse 할 수 있다.
```go
// Read the existing address book.
in, err := ioutil.ReadFile(fname)
if err != nil {
        log.Fatalln("Error reading file:", err)
}
book := &pb.AddressBook{}
if err := proto.Unmarshal(in, book); err != nil {
        log.Fatalln("Failed to parse address book:", err)
}
```

---------------------

# 무엇을 알아야 할까?
- message 를 작성하는 방법
- build 하는 방법
- 주고 받고 하는 방법
- data 파일은 뭔가?
- JSON 파일 <-> 프로토콜 버퍼로 변경 가능?

## data 파일은?
protobuf 로 직렬화된 파일 

## Json 파일 변환
https://godoc.org/github.com/golang/protobuf/jsonpb





# ref
[조대협블로그-정리](https://bcho.tistory.com/1182)
[reference](https://developers.google.com/protocol-buffers/)
[github](https://github.com/protocolbuffers/protobuf)
[goglang/protobuf](https://github.com/golang/protobuf)