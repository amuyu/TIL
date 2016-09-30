
## Selector
셀렉터는 여러 SelectableChannel을 자신에게 등록하게 하고 등록된 SelectableChannel의 이벤트 요청들을 나눠서 적절한 서비스 제공자에게 보내 처리하는 것이다.
## SelectionKey
SelectionKey가 해당 채널이 접속할 준비가 되어 있는지, 읽을 준비가 되었는지 등의 정보를 저장하고 있고 Selector가 이 정보를 보고 준비된 채널들만 처리
특정 채널과 셀렉터 사이의 등록 관계 캡슐화
## SelectableChannel
모든 소켓 채널들의 슈퍼 클래스

### Selector 등록 방법
우선 등록할 채널을 비블록킹 모드 설정 후, SelectableChannel의 register() 메소드를 호출하여 Selector에 등록

### 접속 시도 OP_ACCEPT
SoketChannel : OP_CONNECT, OP_READ, OP_WRITE
DatagramChannel : OP_READ, OP_WRITE
Pipe.SourceChannel : OP_READ
Pipe.SinkChannel : OP_WRITE

### 구조
```
SelectableChannel - SelectionKey - Selector
```
채널을 셀렉터에 등록하면 해당 채널과 채널이 셀렉터에 등록하기 원하는 이벤트를 캡슐화한 셀렉트키를 만들고 셀렉트키를 셀렉트가 관리하는 내부 Set에 추가, 셀렉션키는 셀렉터와 채널 사이의 중계자 역할

## 예제
### 서버
1. 채널들을 관리할 Selector를 얻는다.
```java
Selector selector=Selector.open();
```
2. ServerSocketChannel를 얻는다.
```java
ServerSocketChannel server=ServerSocketChannel.open();
```
3. 내부 소켓을 얻는다.
```java
ServerSocket socket=server.socket();
```
4. binding 한다.
```java
SocketAddress addr=new InetSocketAddress(포트번호);
socket.bind(addr);
```
5. ServerSocketChannel을 Selector에 등록시킨다. ServerSocket는 OP_ACCEPT동작만 할 수 있다.
```java
server.register(selector, SelectionKey.OP_ACCEPT);
```
6. 클라이언트의 접속을 대기한다. 이때 접속이 되면 accept()에 의해 상대방 소캣과 연결된 SocketChannel의 인스턴스를 얻는다. 이 채널는 읽기(OP_READ),쓰기(OP_WRITE),접속(OP_CONNECT) 행동을 지원한다.
```java
SocketChannel socketChannel=serverChannel.accept();
```
7. 접속된 SocketChannel를 Selector에 등록한다.
```java
socketChannel.register(selector, 소켓채널의 해당행동);
```
8. 채널이 취할 수 있는 3가지 동작(읽기(OP_READ),쓰기(OP_WRITE),접속(OP_CONNECT) )에 대해서 검사한다. 이때는 다음과 같은 메서드를 이용해서 체크를 한다.
- isConnectable() : true이면 상대방 소켓과 새로운 연결이 됐다는 뜻이다. 이때 Non-Blocking I/O 모드일 경우에는 연결과정이 끝나지 않는 상태에서 리턴될 수도 있다. 그러므로 SocketChannel 의 isConnectionPending()메서드가 true를 리턴하는지 아닌지를 보고 true를 리턴한다면 finishConnection()를 명시적으로 불러서 연결을 마무리짓도록 한다.
- isReadable() : true이면 읽기(OP_READ) 동작이 가능하다는 뜻으로 SocketChannel로부터 데이터를 읽는 것에 관한 정보를 받을 준비가 되었다는 뜻이다. 따라서 버퍼에 읽어들이기만 하면된다.
- isWritable() : true이면 쓰기(OP_WRITE) 동작이 가능하다는 뜻으로 SocketChannel에 데이터를 쓸 준비가 되었다는 뜻이다. 따라서 버퍼의 데이터를 이 채널에 write()하면 된다.  

### 클라이언트
1. SocketChannel를 얻어서 접속한다.
```java
SocketAddress addr=new InetSocketAddress("localhost", 8080);
SocketChannel socket=SocketChannel.open(addr);
```
버퍼를 만들고 서버에서 들어온 데이터를 읽고 쓰기를 한다.


## 참고
[자바 NIO 강좌](http://javaexpert.tistory.com/464)
[셀렉터 부분 정리](http://hjhistory.tistory.com/entry/NIO-%EC%85%80%EB%A0%89%ED%84%B0-%EB%B6%80%EB%B6%84-%EC%A0%95%EB%A6%AC)
[SocketChannel 이용한 non-blocking server/client 예제](http://tjjava.blogspot.kr/2012/09/socketchannel-non-blocking-serverclient.html)
