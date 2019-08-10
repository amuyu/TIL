# json
Go 에서 json 을 사용하기 위해서는 표준패키지 encoding/json 을 사용하면 된다.
패키지 정보는 https://golang.org/pkg/ 에서 확인할 수 있다.

# 인코딩
Go 데이타를 JSON 포맷으로 변환하기 위해서는 encoding/json 패키지의 Marshal 함수를 사용한다.
흔히 Go 구조체 혹은 map 데이타를 JSON 으로 인코딩하게 되는데, 해당 Go 데이타 값을 json.Marshal 로 전달하면
JSON 으로 인코딩된 바이트배열과 에러 객체를 리턴한다.
JSON 으로 인코딩된 바이트배열을 다시 문자열로 변경할 필요가 있다면 string(바이트배열)과 같이 변경할 수 있다.

인코딩 샘플
```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
//Member -
type Member struct {
    Name   string
    Age    int
    Active bool
}
 
func main() {
    // Go 데이타
    mem := Member{"Alex", 10, true}
    // JSON 인코딩
    jsonBytes, err := json.Marshal(mem)
    if err != nil {
        panic(err)
    }
    // JSON 바이트를 문자열로 변경
    jsonString := string(jsonBytes)
    fmt.Println(jsonString)
}
```


# 디코딩
JSON 으로 인코딩된 데이타를 다시 디코딩하기 위해서는 encoding/json 패키지의 Unmarshal() 함수를 사용한다.
Unmarshal() 함수의 첫번째 파라미터에는 JSON 데이타를 두번째 파라미터에는 출력할 구조체를 포인터로 지정한다.
리턴값은 에러객체이고, 에러가 없을 경우, 두번째 파라미터에 원래 데이타가 복원된다.

디코딩 샘플
```go
var mem Member
err := json.Unmarshal(bytes, &mem)
if err != nil {
    panic(err)
}
fmt.Println(mem)

var m map[string]interface{}
err = json.Unmarshal(bytes, &m)
fmt.Println(m)
```

interface{} 는 dyanmic type 으로 여러 자료형에 대해서 처리할 수 있다.
출력 구조체나 map 을 미리 정의하지 않고 선언만 한 상태에서 디코딩할 수 있다.
