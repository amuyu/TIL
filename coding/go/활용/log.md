# 로그 패키지
Go 에서 로그를 사용하기 위해 Go 의 표준 패키지 log 를 이용할 수 있다.

다음과 같이 log 패키지의 Print* 출력함수를 호출하면 로그 내용을 화면에 출력한다.

```go
func main() {
// log.SetFlags(0)
log.Println(“Logging”)
}
```

Log 패키지는 여러가지 로거를 지원하기 위해 Logger 라는 타입(인터페이스) 를 제공하고 있다. 새로운 Logger를 만들기 위해 log.New() 함수를 호출한다.

log.New 함수의 첫번째 파라미터는 io.Writer 인터페이스를 지원하는 타입으로 os.Stdout, os.Stderr, fp 등이 될 수 있다. 두번째 파라미터는 로그출력의 가장 처음에 적는 prefix 로서 프로그램명, 카테고리 등을 기재할 수 있다. 세번째 파라미터는 로그 플래그로 log.LstdFlags, log.Ldate, log.Ltime, log.Lshortfile, log.Llongfile 등을 | 로 묶어 지정할 수 있다.

아래 예제는 Stdout 으로 로그를 보내는 logger 를 만들어 로깅하는 코드이다.

```go

import (
	"log"
	"os"
)

var myLogger *log.Logger

func main() {
	myLogger = log.New(os.Stdout, "INFO ::: ", log.LstdFlags)
	myLogger.Println("logging")
}
```


# 파일로그
파일에 로깅하기 위해서는 먼저 로그 파일을 오픈하고 파일포인터를 log.New() 첫번째 파라미터에 넣어주면 된다. 로그 파일을 오픈할 때는 일반적으로 Write Only, Append Only 모드로 오픈한다.

```go
import (
	"log"
	"os"
)

var myLogger *log.Logger

func main() {
	fpLog, err := os.OpenFile("logFile.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
	if err != nil {
		panic(err)
	}
	defer fpLog.Close()

	myLogger = log.New(fpLog, "File :::", log.LstdFlags)
	myLogger.Println("file logging")
}
```

로그파일을 사용하는 좀 더 간편한 방법으로 아래와 같이 표준로거를 파일로거로 변경하는 방식이 있다. 이 방식의 장점은 별도의 Logger를 전역 변수에 지정할 필요가 없으며, log.Print 같은 log 메서드를 그대로 사용하면 된다.

```go
fpLog, err := os.OpenFile("logfile.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
if err != nil {
panic(err)
}
defer fpLog.Close()

log.SetOutput(fpLog)
```

복수타겟 로깅

복수 개의 로그 타겟에 동시에 로그를 출력하기 위해서 io.MultiWriter 를 사용할 수 있다. Io.MultiWriter() 에 여러 Io.Writer 들을 지정하여 복수 writer 를 만들고 log.New() 혹은 log.SetOutput() 에 지정하면, 복수 타겟에 로그를 쓰게 된다.


# oreo

log 패키지를 사용하면 logging 할 수 있다.

log import 후, `log.Println(message)` 를 호출하면 로그 내용을 화면에 출력한다.

log 패키지에서 제공하는 Logger 타입을 사용하면 여러 형태로 로깅할 수 있다.

`log.New()` 라는 메서드로 Logger 를 생성하며 로그 flag, log prefix,
log writer 각각 설정할 수 있다. 

log writer 를 파일 pointer 로 변경하면 파일에도 로깅할 수 있다.

복수 개의 로그 타겟에 로그를 출력하려면 `io.MultiWriter` 를 사용한다.

