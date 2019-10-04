# 변수
## 선언하는 방법
변수는 var 를 사용하여 선언한다. var 키워드 뒤에 변수명을 적고, 그 뒤에 타입을 적는다.
```go
var a int = 1
var i, j, k int = 1, 2, 3
```
변수를 선언하면서 초기값을 지정하지 않으면, Go는 Zero Value를 기본적으로 할당한다.
숫자형에는 0, bool 타입에는 false, string 형에는 "" 을 할당한다.
Go 에서는 할당되는 값을 보고 그 타입을 추론하는 기능이 있다.

## short assignment 사용
`short assignment statement`(:=) 를 사용해 변수를 선언할 수 있다. var를 생략할 수 있다.
func 내에서만 사용할 수 있다. 함수 밖에서는 var를 사용해야 한다.

## 주의
만약 선언된 변수가 Go 프로그램 내에서 사용되지 않는다면, 에러를 발생시킨다.

# 상수
const를 사용하여 선언한다.
```go
const c int = 10
const c = 10	// 값을 보고 타입 추론
// 여러 개의 상수를 묶어서 지정할 수 있고, 괄호 안에 상수들을 나열한다.
const (
    Visa = "Visa"
    Master = "MasterCard"
    Amex = "American Express"
)
```
`iota` 라는 identifier를 사용하면 상수값을 순차적으로 부여할 수 있다.
```go
const (
    Apple = iota // 0
    Grape        // 1
    Orange       // 2
)
```

# keyword
Go에서 사용하는 예약 키워드들이다.
```go
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

# type
- 부울린 타입
bool
- 문자열 타입
string: string은 한번 생성되면 수정될 수 없는 Immutable 타입임
- 정수형 타입
int int8 int16 int32 int64
uint uint8 uint16 uint32 uint64 uintptr
- Float 및 복소수 타입
float32 float64 complex64 complex128
- 기타 타입
byte: uint8과 동일하며 바이트 코드에 사용
rune: int32과 동일하며 유니코드 코드포인트에 사용한다

# 문자열
문자열 리터럴은 Back Quote(` `) 혹은 이중인용부호(" ")를 사용하여 표현할 수 있다.
- Back Quote 는 Raw String Literal 이라고 부른다. raw string 그대로의 값을 갖는다. 예를 들어 문자열 안에 \n 이 있을 경우 NewLine 으로 해석되지 않는다.
- 이중인용부호는 일반적으로 사용하는 string

# type conversion
데이터 타입을 변환하기 위해서는 `T(v)` 와 같이 표현한다. 이를 Type Conversion 이라 부른다.
여기서 T는 변환하고자 하는 타입을 표시하고, v는 변환될 값을 지정한다. 
예를 들어, 정수 100을 float 으로 변경하기 위해서 float32(100) 으로 표현하고,
문자열을 바이트배열로 변경하기 위해서 []byte("ABC") 처럼 표현할 수 있다.
아래는 여러 Type Conversion 의 예를 보인 것이다.
```go
func main() {
    var i int = 100
    var u uint = uint(i)
    var f float32 = float32(i)  
    println(f, u)

    str := "ABC"
    bytes := []byte(str경
    str2 := string(bytes)
    println(bytes, str2)
}
```

# 연산자
- 산술연산자 : +, -, \*, /, %, ++,—
- 관계연산자
```go
a == b
a != c
a >= b
```
- 논리연산자
```go
A && B
A || !(C && B)
```
- Bitwise 연산자
```go
c = (a & b) << 5
```
- 할당연산자 : =, +=, &=, <<=
- 포인터연산자 : &, * 를 사용하여 변수의 주소를 얻어내고 Dereference할 때 사용, 포인터 산술은 안된다.

# 조건문
## if
if 문은 해당 조건이 맞으면 { } 블럭안의 내용을 실행한다.
조건식을 괄호 ( )로 둘러 싸지 않아도 된다.
반드시 Boolean 식으로 표현되어야 한다.
```go
if k == 1 {
    println("One")
} else if k == 2 {  //같은 라인
    println("Two")
} else {   //같은 라인
    println("Other")
}
```
if 문에서 조건식을 사용하기 이전에 간단한 (Optional Statement)를 함께 실행할 수 있다.
아래 예제에서는 val := i*2 라는 문장을 조건식 이전에 실행
```go
if val := i * 2; val < max {
    println(val)
}

// 아래 처럼 사용하면 Scope 벗어나 에러
val++
```

# switch 문
여러 값을 비교해야 하는 경우 혹은 다수의 조건식을 체크해야 하는 경우 switch 문을 사용한다.
다른 언어들과 비슷하게 switch 문 뒤에 하나의 변수(expression)을 지정하고, 
case문에 해당 변수가 가질 수 있는 값들을 지정하여, 각 경우에 다른 블럭들을 실행할 수 있다.
복수개의 case 가 있는 경우, 아래처럼 case 3,4 콤마를 사용해서 나열할 수 있다. 
```go
package main
 
func main() {
    var name string
    var category = 1
 
    switch category {
    case 1:
        name = "Paper Book"
    case 2:
        name = "eBook"
    case 3, 4:
        name = "Blog"
    default:
        name = "Other"
    }
    println(name)
     
    // Expression을 사용한 경우
    switch x := category << 2; x - 1 {
        //...
    }  
}
```
switch 에서 case 문은 case 블럭 마지막에 break 문을 생략해도 break하여 switch문을 빠져나온다.
default break 문을 사용하지 않고 다음 case 문을 실행하게 하려면 fallthrough 문을 명시하면 된다.

go 에서 switch문의 한가지 특징적잉 용법은 switch 문 다음에 조건변수 혹은 expression을 적지 않는 것이다.
이 경우, 각 case 조건문들을 순차적으로 검사하여 조건에 맞는 블럭을 실행한다.
if.. else if.. else if.. 반복 문장을 단순화 할 수 있다.

go 의 또다른 용법은 변수의 타입을 검사하는 타입 switch 가 있다.

# for 문
Go는 반복문에 for 하나 밖에 없다. 다른 언어와 같은 형식으로 작성하되 괄호는 생략한다.
괄호를 쓰면 에러가 발생한다.
```go
package main

func main() {
    sum := 0
    for i := 1; i <= 100; i++ {
        sum += i
    }
    println(sum)
}
```
조건식만 쓰는 for 루프, 초기값과 증감식을 생략해서 for 루프가 다른 언어의 while루프와 같이 쓰이도록 한다.
```go
package main

func main() {
    n := 1
    for n < 100 {
        n *= 2      
        //if n > 90 {
        //   break
        //}     
    }
    println(n)
}
```
for 루프로 무한루프를 만들려면 초기값, 조건식, 증감식을 모두 생략하면 된다.

for range 문은 컬렉션으로부터 한 요소씩 가져와 차례로 for 블럭의 문장을 실행한다.
foreach와 비슷한 용법이다. `for 인덱스, 요소값 := range 컬렉션`
```go
names := []string{"홍길동", "이순신", "강감찬"}

for index, name := range names {
    println(index, name)
}
```


# break, continue, goto 문
for 루프에서 빠져나올때는 break
for 문장에서 나머지 부분을 실행하지 않고 시작부분으로 가는 continue
기타 임의의 문자으로 이동하는 goto 문


# 함수
Go에서 함수는 func 키워드를 사용하여 정의한다.
각 파라미터는 파라미터명 뒤에 파라미터 타입을 적어 정의한다.
함수의 리턴 타입의 괄호 뒤에 적는다.
```go
package main
func main() {
    msg := "Hello"
    say(msg)
}

func say(msg string) {
    println(msg)
}
```
## 파라미터 전달
Go에서 파라미터를 전달하는 방식은 Pass By Value 와 Pass By Reference로 나뉜다.
- Pass By Value : 변수 값 전달
- Pass By Reference : 변수 주소 전달

## Variadic (가변인자함수)
다양한 숫자의 파라미터를 전달하고자 할 때 `…` 3개의 마침표를 사용한다.
```go
package main
func main() {   
    say("This", "is", "a", "book")
    say("Hi")
}

func say(msg ...string) {
    for _, s := range msg {
        println(s)
    }
}
```

## 함수 리턴값
Go에서 함수는 리턴값이 없을 수도, 하나일 수도, 복수 개일 수도 있다.
Named Return Parameter 라는 기능을 제공한다. 리턴되는 값들을 리턴 파라미터들에
할당할 수 있는 기능이다. 리턴 타입이 여러개일때 가독성을 높일 수 있다.
named return parameter 에 값을 할당하고 리턴할 때는 빈 return 을 적어준다.
```go
func sum(nums ...int) (count int, total int) {
    for _, n := range nums {
        total += n
    }
    count = len(nums)
    return
}
```
복수개의 타입을 리턴할 경우에는 리턴 타입들을 괄호 ()안에 적어준다.

# 익명함수
함수명을 갖지 않는 함수를 익명함수라 부른다. 일반적으로 익명함수는 함수 전체를 변수에 할당하거나
다른 함수의 파라미터에 직접 정의되어 사용되곤 한다.
익명함수는 func 뒤에 함수명을 생략한다
```go
package main
 
func main() {
    sum := func(n ...int) int { //익명함수 정의
        s := 0
        for _, i := range n {
            s += i
        }
        return s
    }
 
    result := sum(1, 2, 3, 4, 5) //익명함수 호출
    println(result)
}
```


# 일급함수
Go 언어에서 함수는 일급함수로서 Go의 기본 타입과 동일하게 취급되며,
따라서 다른 함수의 파라미터로 전달하거나 리턴값으로도 사용될 수 있다.

# Named Type
자료형에 새로 이름을 붙일 수 있다.
```
type MyInt int
```

# type 문을 사용한 함수 원형 정의
type 문은 구조체struct), 인터페이스 등 Custom Type 을 정의하기 위해 사용한다.
type 문은 또한 함수의 원형을 정의하는데 사용될 수 있다. 예를 들어
`func(int, int) int` 함수 원형이 반복된다고 할 때 다음과 같이 정의할 수 있다.
```
type calculator func(int, int) int

func calc(f calculator, a int, b int) int {
    result := f(a, b)
    return result
}
```
이렇게 함수의 원형을 정의하고 함수를 타 메서드에 전달하고 리턴받는 기능을
타 언어에서 흔히 델리게이트(delegate)라 부른다. Go는 이러한 delegate 기능을 가지고 있다.

# Closure
Closure는 함수 바깥에 있는 변수를 참조하는 함수값을 일컫는데,
바깥의 변수를 마치 함수 안으로 끌어들인 듯이 그 변수를 읽거나 쓸 수 있게 된다.
```go
package main

func nextValue() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}

func main() {
    next := nextValue()

    println(next())  // 1
    println(next())  // 2
    println(next())  // 3

    anotherNext := nextValue()
    println(anotherNext()) // 1 다시 시작
    println(anotherNext()) // 2
}
```

# Array
배열은 연속적인 메모리 공간에 동일한 타입의 데이터를 순서적으로 저장하는 자료구조이다.
배열의 선언은 `var 변수명 [배열크기] 데이터타입` 과 같이 한다.
배열의 크기를 데이터 타입 앞에 써주는 것이 c나 java 와는 다르다.
Go 에서 배열의 크기는 Type 을 구성하는 한 요소이다.
[3]int와 [5]int 는 서로 다르다. 배열이 선언된 이후에 각 배열의 요소를 인덱스를 사용하여 읽거나 쓸 수 있다.
```go
package main

func main() {
    var a [3]int  //정수형 3개 요소를 갖는 배열 a 선언
    a[0] = 1
    a[1] = 2
    a[2] = 3
    println(a[1]) // 2 출력
}
```

## 배열의 초기화
배열을 정의할 때, 초기값을 설정할 수 도 있다.
배열 선언 뒤에 { } 괄호를 두고 초기값을 순서대로 적으면 된다
[...] 을 사용하여 배열크기를 생략하면 자동으로 초기화 숫자만큼 배열 크기가 정해진다.
```go
var a1 = [3]int{1, 2, 3}
var a3 = [...]int{1, 2, 3} //배열크기 자동으로
```

## 다차원 배열의 초기화
단차원 배열의 초기화와 비슷하다. 다만 다차원이므로 배열 초기값 안에 다시
배열값을 넣는 형태를 띈다.
```go
func main() {
    var a = [2][3]int{
        {1, 2, 3},
        {4, 5, 6},  //끝에 콤마 추가
    }
    println(a[1][2])
}
```

## slice
Go 배열은 고정된 배열 크기 안에 동일한 데이터 타입을 연속으로 저장하지만,
배열의 크기를 동적으로 증가시키거나 부분 배열을 발췌하는 등의 기능을 가지고 있지 않다.
Go slice 는 배열의 이런 제약점을 넘어 다양한 기능을 제공한다.

슬라이스 선언은 배열을 선언하듯이 `var v []T` 처럼 한다. 배열의 크기는 지정하지 않는다.
```go
package main
import "fmt"

func main() {
    var a []int        //슬라이스 변수 선언
    a = []int{1, 2, 3} //슬라이스에 리터럴값 지정
    a[1] = 10
    fmt.Println(a)     // [1, 10, 3]출력
}
```
slice 를 생성하는 방법은 내장함수 make()를 이용할 수 있다.
make 함수로 슬라이스를 생성하면 개발자가 슬라이스의 길이와 용량을 임의로 지정할 수 있는 장점이 있다.
make 첫번째 파라미터에 슬라이스의 타입을,
두번째 파라미터에 슬라이스의 길이,
세번째 파라미터에 capacity 를 지정한다.
```go
func main() {
    s := make([]int, 5, 10)
    println(len(s), cap(s)) // len 5, cap 10
}
```
슬라이스에 길이와 용량을 지정하지 않으면 기본적으로 길이와 용량이 0인 슬라이스를 만든다.
이를 Nil Slice 라고 부른다. nil 과 비교하면 참이 된다.


## sub-slice
슬라이스에서 일부를 발췌하여 부분 슬라이스를 만들 수 있다.
`슬라이스[처음인덱스:마지막인덱스]` 형식으로 만든다.
처음인덱스는 inclusive 이고 마지막 인덱스는 exclusive 로 python 과 동일하다.
```go
package main
import "fmt"

func main() {
    s := []int{0, 1, 2, 3, 4, 5}
    s = s[2:5]  
    fmt.Println(s) //2,3,4 출력
}

s := []int{0, 1, 2, 3, 4, 5}
s = s[2:5]     // 2, 3, 4
s = s[1:]      // 3, 4
fmt.Println(s) // 3, 4 출력
```
슬라이스 인덱스는 처움/마지막 둘 중 하나 혹은 둘 다를 생략할 수 있다.
0부터 4인데스까지 포함하기 위해서는 [:5] 
2부터 마지막 인덱스까지 포함하기 위해서는 [2:]
[:]는 전체를 포함한다.

## 슬라이스 추가, 병합, 복사
배열은 고정된 크기로 그 크기 이상의 데이터를 추가할 수 없지만
슬라이스는 자유롭게 새로운 요소를 추가할 수 있다.
슬라이스에 새로운 요소를 추가하기 위해서는 append() 를 사용한다.
첫 파라미터는 슬라이스 객체이고, 두번째는 추가할 요소의 값이다.
```go
func main() {
    s := []int{0, 1}

    // 하나 확장
    s = append(s, 2)       // 0, 1, 2
    // 복수 요소들 확장
    s = append(s, 3, 4, 5) // 0,1,2,3,4,5

    fmt.Println(s)
}
```
내장함수 append 가 슬라이스에 데이터를 추가할 때, 내부적으로 다음과 같은 일이 일어난다.
슬라이스 용량(capacity)가 남아있는 경우에는 length 를 변경하여 데이터를 추가하고
capacity 를 초과하는 경우에는 현재 용량의 2배에 해당하는 새로운 underlying array를 생성하고
기존 배열 값들을 모두 새로운 배열에 복사한뒤 다시 슬라이스를 할당한다.

copy() 를 사용하여 한 슬라이스를 다른 슬라이스로 복사할 수도 있다.

```go
func main() {
    source := []int{0, 1, 2}
    target := make([]int, len(source), cap(source)*2)
    copy(target, source)
    fmt.Println(target)  // [0 1 2 ] 출력
    println(len(target), cap(target)) // 3, 6 출력
}
```


# 슬라이스 내부 구조
슬라이스는 capacity 크기의 배열이 있고
배열의 부분영역인 세그먼트에 대한 정보를 가지고 있다. 
첫번째 필드는 배열의 포인터에 대한 정보
두번째 필드는 세그먼트의 길이를
세번째 필드는 세그먼트의 최대길이를 가진다.


# Map
Map 은 Hash table을 구현한 자료구조이다.
`map[Key타입]Value타입` 과 같이 선언할 수 있다.

선언만 하는 경우 nil map 이라고 부르고 nil map 에는 어떤 데이터도 쓸 수 없다.
map 을 초기화하는 방법은 make() 함수를 사용하거나 리터럴을 사용하는 방법이 있다.
```go
var idMap map[int]string
idMap = make(map[int]string)	// map 초기화
//리터럴을 사용한 초기화
tickers := map[string]string{
    "GOOG": "Google Inc",
    "MSFT": "Microsoft",
    "FB":   "FaceBook",
}
```

## Map 사용
map 에 새로운 데이터를 추가하기 위해서는 "map변수[키] = 값"과 같이 해당 키에 값을 할당하면 된다.
특정키의 값을 읽을 때는 "map변수[키]"를 읽으면 된다.
특정키를 삭제할 때는 delete를 사용하면 된다.

## Map 키 체크
map 안에 특정 키가 존재하는제 체크
`map변수[키]` 읽기를 수행할 때 2개의 리턴값을 리턴한다. 첫번째는 키에 상응하는 값이고,
두번째는 그 키가 존재하는지 아닌지를 나타내는 bool값이다.
```go
package main

func main() {
    tickers := map[string]string{
        "GOOG": "Google Inc",
        "MSFT": "Microsoft",
        "FB":   "FaceBook",
        "AMZN": "Amazon",
    }

    // map 키 체크
    val, exists := tickers["MSFT"]
    if !exists {
        println("No MSFT ticker")
    }
}
```
Map 이 갖고 있는 모든 요소들을 출력하기 위해, for range 루프를 사용할 수 있다.


# 패키지
Go는 패키지를 통해 코드의 모듈화, 코드의 재사용 기능을 제공한다.
Go는 패키지를 사용해서 작은 단위의 컴포넌트를 작성하고, 이러한 작은 패키지들을 활용해서 프로그램을 작성할 것을 권장한다.

Go에 사용하는 표준패키지는 https://golang.org/pkg 에 자세히 설명되어 있다

## main
일반적으로 패키지는 라이브러리로서 사용되지만, "main" 이라고 명명된 패키지는 Go Compiler 에 의해 특별하게 인식된다. 컴파일러는 main 패키지를 실행 프로그램으로 만든다.

## 패키지 import
다른 패키지를 프로그램에서 사용하기 위해서는 import 를 사용하여 패키지를 포함시킨다.
패키지를 import할 때, 컴파일러는 GOROOT or GOPATH 환경변수를 검색한다.

GOROOT 는 Go 설치시 자동으로 시스템에 설정되지만, GOPATH는 사용자가 지정해 주어야 한다.

## 패키지 Scope
패키지 내에는 함수, 구조체, 인터페이스, 메서드 등이 존재하는데 이름이 첫글자가 대문자로 시작하면
public으로 사용할 수 있다. 소문자로 시작하면 non-public 으로 패키지 내부에서만 사용한다.

## 패키지 init 함수와 alias
패키지 실행시 처음으로 호출되는 init() 함수를 작성할 수 있다. 패키지가 로드되면서 실행되는 함수로 별도의 호출 없이 자동으로 호출된다.
```go
package testlib

var pop map[string]string

func init() {   // 패키지 로드시 map 초기화
    pop = make(map[string]string)
}
```

경우에 따라 패키지를 import 하면서 init() 함수만을 호출하고자 하면 패키지 import 시
`_` 라는 alias 를 지정한다.
```go
package main
import _ "other/xlib"
```
만약 패키지 이름이 동일하지만, 서로 다른 버전 혹은 서로 다른 위치에서 로딩하고자 할 때는 패키지 alias 를 사용해서 구분할 수 있다.
```go
import (
    mongo "other/mongo/db"
    mysql "other/mysql/db"
)
func main() {
    mondg := mongo.Get()
    mydb := mysql.Get()
}
```


## 사용자 정의 패키지 생성
사용자 정의 라이브러리 패키지는 일반적으로 폴더를 하나 만들고 그 폴더 안에 .go 파일들을 만들어 구성한다.
하나의 서브 폴더안에 있는 .go 파일들은 동일한 패키지명을 갖는다.
패키지명은 해당 폴더의 이름과 같게 한다.

예제로 간단한 패키지를 만들어보자 24lab.net/testlib 폴더를 생성한 후에 music.go 라는 파일을 작성한다.
```go
package testlib
 
import "fmt"
 
var pop map[string]string
 
func init() {
    pop = make(map[string]string)
    pop["Adele"] = "Hello"
    pop["Alicia Keys"] = "Fallin'"
    pop["John Legend"] = "All of Me"
}
 
// GetMusic : Popular music by singer (외부에서 호출 가능)
func GetMusic(singer string) string {
    return pop[singer]
}
 
func getKeys() {  // 내부에서만 호출 가능
    for _, kv := range pop {
        fmt.Println(kv)
    }
}
```

Optional : 사이즈가 큰 복잡한 라이브러리 같은 경우 `go install` 명령을 사용하여
라이브러리를 컴파일하여 Cache 할 수 있다. 이렇게 하면 /pkg 폴더안에 컴파일된 파일이 생성된다.

이렇게 정의한 패키지를 사용하려면 다음과 같이 작성한다.
```go
package main

import "24lab.net/testlib"

func main() {
    song := testlib.GetMusic("Alicia Keys")
    println(song)
}
```
여기 코드에선 25lab.net/testlib 패키지를 import 하고 해당 패키지의 Export 함수인 GetMusic() 을 호출하고 있다.

Go는 24lab.net/testlib 패키지를 찾기 위해 GOROOT와 GOPATH 를 찾는다.
GOPATH 가 C:\GoApp;C:\GoSrc 인 경우, 지정된 라이브러리를 찾기 위해 다음과 같은
폴더를 순차적으로 검색한다.
```
C:\Go\src\24lab.net\testlib (from $GOROOT)
C:\GoApp\src\24lab.net\testlib (from $GOPATH)
C:\GoSrc\src\24lab.net\testlib
```

# Struct
Go에서 struct 는 Custom Data Type을 표현한는데 사용한다. Go의 struct는 필드들의 집합체이며 필드들의 컨테이너이다. Go에서 struct는 필드 데이타만을 가지며, 메서드를 가지지 않는다.

Go 언어는 객체지향 프로그래밍을 고유의 방식으로 지원한다. Go에는 전통적인 OOP 언어가 가지는 클래스, 객체, 상속 개념이 없다. 전통적인 OOP의 클래스는 Go 언어에서 Custom 타입을 정의하는 struct로 표현한다. Go 언어의 struct는 필드만을 가지며, 메서드는 별도로 분리해서 정의한다.

struct 를 정의하기 위해서 type 문을 사용한다.
```go
package main

import "fmt"

// struct 정의
type person struct {
    name string
    age  int
}

func main() {
    // person 객체 생성
    p := person{}

    // 필드값 설정
    p.name = "Lee"
    p.age = 10

    fmt.Println(p)
}
```

## 객체 생성
struct 타입으로부터 객체를 생성하는 방법은 몇 가지 방법들이 있다.
위의 예제처럼 person{} 를 사용하여 빈 person 객체를 먼저 할당하고, 나중에 필드값을 채워넣는 방법이 있다.

struct 객체를 생성할 때, 초기값을 함께 할당하는 방법도 있다.
아래 예처럼, struct 필드값을 순서적으로 {} 괄호안에 넣을 수 있으며, 두번째 예처럼 순서에 상관없이 필드명을 지정하고 named field 그 값을 넣을 수도 있다.
```go
var p1 person
p1 = person{"Bob", 20}
p2 := person{name: "Sean", age: 50}
p := new(person)
```

또 다른 생성 방법으로는 Go 내장함수 new()를 쓸 수 있다.
new()를 사용하면 모든 필드를 Zero value로 초기화하고 person 객체의 포인터(*person)을 리턴한다. 객체 포인터인 경우에도 필드 액세스시, .(dot)을 사용하는데, 이 때 포인터는 자동으로 Dereference 된다.
```go
p := new(person)
p.name = "Lee"
```
Go에서 struct는 기본적으로 mutable 개체로서 필드값이 변화할 경우, 해당 개체 메모리에 직접 변경된다. 하지만 struct 개체를 다른 함수의 파라미터로 넘긴다면, Pass by value 에 따라 객체를 복사해서 전달한다. Pass by Reference 로 struct 를 전달하고자 한다면 struct의 포인터를 전달해야 한다.

## 생성자 함수
struct 의 필드가 사용 전에 초기화되어야 하는 경우가 있다.
이러한 목적을 위해 생성자 함수를 사용할 수 있다.
```go
package main

type dict struct {
    data map[int]string
}

//생성자 함수 정의
func newDict() *dict {
    d := dict{}
    d.data = map[int]string{}
    return &d //포인터 전달
}

func main() {
    dic := newDict() // 생성자 호출
    dic.data[1] = "A"
}
```

# Method
Go 언어는 객체지향 프로그래밍을 고유의 방식으로 지원한다.
Go 메서드는 특별한 형태의 func 함수이다.
메서드는 함수 정의에서 fund 키워드와 함수명 사이에
그 함수가 어떤 struct를 위한 메서드인지를 표시한다.

흔히 receiver 로 불리우는 이 부분은 메서드가 속한 struct 타입과
struct 변수명을 지정하는데 struct 변수명은 함수 내에서 입력 파라미터처럼 사용한다.

```go
package main

//Rect - struct 정의
type Rect struct {
    width, height int
}

//Rect의 area() 메소드
func (r Rect) area() int {
    return r.width * r.height   
}

func main() {
    rect := Rect{10, 20}
    area := rect.area() //메서드 호출
    println(area)
}
```

## value vs pointer receiver
Value receiver 는 struct의 데이터를 복사하여 전달하며, 포인터 receiver는 struct의 포인터만을 전달한다.

Value receiver 의 경우 메서드 내에서 struct의 필드값이 변경되더라도
호출자의 데이터는 변경되지 않는 반면, pointer receiver는
메서드 내의 필드값 변경이 그대로 호출자에서 반영된다.

아래와 같이 포인터 receiver 로 변경하면 area2 메서드 내에서 r.width++ 필드 변경분이 main() 함수에도 반영되기 때문에 출력값이 변한다.
```go
// 포인터 Receiver
func (r *Rect) area2() int {
    r.width++
    return r.width * r.height
}

func main() {
    rect := Rect{10, 20}
    area := rect.area2() //메서드 호출
    println(rect.width, area) // 11 220 출력
}
```

# 인터페이스
struct가 필드들의 집합체라면, interface 는 type이 구현해야 하는 메서드들의 집합체이다.
인터페이스는 struct와 마찬가지로 type문을 사용하여 정의한다.
```go
type Shape interface {
    area() float64
    perimeter() float64
}
```

## 인터페이스 구현
인터페이스를 구현하기 위해서는 해당 타입이 그 인터페이스의 메서드들을 모두 구현하면 되므로, 위의 Shape 인터페이스를 구현하기 위해서는 area, perimeter 2개의 메서드를 구현하면 된다.
```go
//Rect 정의
type Rect struct {
    width, height float64
}

//Rect 타입에 대한 Shape 인터페이스 구현
func (r Rect) area() float64 { return r.width * r.height }
func (r Rect) perimeter() float64 {
     return 2 * (r.width + r.height)
}
```

## 인터페이스 사용
인터페이스를 사용하는 일반적인 예로 함수가 파라미터로 인터페이스를 받아들이는 경우를 들 수 있다. 함수 파라미터가 인터페이스인 경우, 이는 어떤 타입이든 해당 인터페이스를 구현하기만 하면 모두 입력 파라미터로 사용될 수 있다는 것을 의미한다.
```go
func main() {
    r := Rect{10., 20.}
    c := Circle{10}

    showArea(r, c)
}

func showArea(shapes ...Shape) {
    for _, s := range shapes {
        a := s.area() //인터페이스 메서드 호출
        println(a)
    }
}
```

## 인터페이스 타입
Go의 모든 Type은 적어도 0개의 메서드를 구현하므로, 흔히 Go에서 모든 Type 을 나타내기 위해 빈 인터페이스를 사용한다.
빈 인터페이스를 어떠한 타입도 담을 수 있는 컨테이너라고 볼 수 있다.
빈 인터페이스는 interface{} 와 같이 표현한다. 여러 다른 언어에서 흔히 일컫는 Dynamic Type 이라고 볼 수 있다.

## Type Assertion
Interface type의 x 와 타입 T에 대하여 x.(T)로 표현했을 때,
x가 nil이 아니며, x가 T타입에 속한다는 점을 확인(assert)하는 것으로 이러한 표현을 Type Assertion 이라 부른다.x가 nil 이거나 x의 타입이 T가 아니라면, 런타임 에러가 발생하고 x가 T타입인 경우는 T타입의 x를 리턴한다.
```go
func main() {
    var a interface{} = 1

    i := a       // a와 i 는 dynamic type, 값은 1
    j := a.(int) // j는 int 타입, 값은 1

    println(i)  // 포인터주소 출력
    println(j)  // 1 출력
}
```

# Go 에러
Go는 내장 타입으로 error라는 interface 타입을 갖는다.
개발자는 이 인터페이스를 구현하는 커스텀 에러 타입을 만들 수 있다.
```go
type error interface {
    Error() string
}
```
기본 에러 처리
```go
package errors

// New returns an error that formats as the given text.
func New(text string) error {
	return &errorString{text}
}

// errorString is a trivial implementation of error.
type errorString struct {
	s string
}

func (e *errorString) Error() string {
	return e.s
}
```

# defer 지연실행
defer는 특정 문장 혹은 함수를 나중에 실행하게 한다
일반적으로 defer는 c#, java 같은 언어에서의 finally 블럭처럼 마지막에 Clean-up 작업을 위해 사용된다.

아래 예제는 파일을 Open 한후 바로 파일을 Close 하는 작업을 defer 로 쓰고 있다. 이는 차후 문장에서 어떤 에러가 발생하더라도 항상 파일을 Close 할 수 있도록 한다.
```go
package main
 
import "os"
 
func main() {
    f, err := os.Open("1.txt")
    if err != nil {
        panic(err)
    }
 
    // main 마지막에 파일 close 실행
    defer f.Close()
 
    // 파일 읽기
    bytes := make([]byte, 1024)
    f.Read(bytes)
    println(len(bytes))
}
```

# panic
panic은 현재 함수를 멈추고 현재 함수에 defer 함수들을 모두 실행한 후, 즉시 리턴한다.

이러한 panic 모드 실행 박식은 다시 상위함수에도 똑같이 적용되고, 계속 콜스택을 타고 올라가며 적용된다. 그리고 마지막에는 프로그램이 에러를 내고 종료하게 된다.

# recover
panic 함수에 의한 패닉상태를 정상상태로 되돌린다
```go
package main

import (
    "fmt"
    "os"
)

func main() {
    openFile("1.txt")
    println("Done") // 이 문장 실행됨
}

func openFile(fn string) {
    // defere 함수. panic 호출시 실행됨
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("OPEN ERROR", r)
        }
    }()

    f, err := os.Open(fn)
    if err != nil {
        panic(err)
    }

    // 파일 close 실행됨
    defer f.Close()
}
```

# Gi 루틴
Go 런타임이 관리하는 경량 쓰레드 이다. `go` 키워드를 사용하여
함수를 호출하면, 런타임시 새로운 groutine 을 실행한다. goroutine 은 비동기적으로 함수루틴을 실행한다.
```go
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 10; i++ {
        fmt.Println(s, "***", i)
    }
}

func main() {
    // 함수를 동기적으로 실행
    say("Sync")

    // 함수를 비동기적으로 실행
    go say("Async1")
    go say("Async2")
    go say("Async3")

    // 3초 대기
    time.Sleep(time.Second * 3)
}
```

## 익명함수
고루틴은 익명함수를 실행할 수도 있다.
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    // WaitGroup 생성. 2개의 Go루틴을 기다림.
    var wait sync.WaitGroup
    wait.Add(2)

    // 익명함수를 사용한 goroutine
    go func() {
        defer wait.Done() //끝나면 .Done() 호출
        fmt.Println("Hello")
    }()

    // 익명함수에 파라미터 전달
    go func(msg string) {
        defer wait.Done() //끝나면 .Done() 호출
        fmt.Println(msg)
    }("Hi")

    wait.Wait() //Go루틴 모두 끝날 때까지 대기
}
```
여기서 sync.WaitGroup을 사용하고 잇는데, 이는 기본적으로 여러 Go 루틴들이 끝날 때까지 기다리는 역할을 한다. 먼저 Add() 메소드에 몇개의 Go루틴을 기다릴 것인지 지정하고, 각 Go루틴에서 Done() 메서드를 호출한다. 그리고 메인 루틴에서 Wait() 메서드를 호출해서 Go 루틴들이 모두 끝나기를 기다린다.


## 다중 CPU 처리
CPU에서 병렬처리 하게 설정할 수 있다.
```go
package main

import (
    "runtime"  
)

func main() {
    // 4개의 CPU 사용
    runtime.GOMAXPROCS(4)

    //...
}
```

# Go 채널
채널을 통해 데이터를 주고받을 수 있다.
make() 함수를 통해 생성되고 채널 연산자 `<-`을 통해 데이터를 보내고 받는다.
채널은 고루틴들 사이 데이터를 주고 받는데 사용한다.
상대편이 준비될 때까지 채널에서 대기함으로써 별도의 lock을 걸지않고
데이터를 동기화하는데 사용한다.

채널로 데이터를 보낼 때는 `채널명 <- 데이터` 와 같이 하고
채널로부터 데이터를 받을 경우는 `<- 채널명` 과 같이 사용한다.

```go
package main

func main() {
  // 정수형 채널을 생성한다
  ch := make(chan int)

  go func() {
    ch <- 123   //채널에 123을 보낸다
  }()

  var i int
  i = <- ch  // 채널로부터 123을 받는다
  println(i)
}
```
Go 채널은 수신자와 송신자가 서로를 기다리는 속성때문에,
Go 루틴이 끝날 때까지 기다리는 기능을 구현할 수 있다.

## Go 채널 버퍼링
Unbuffered Channel과 Buffered Channel 이 있다.
Unbuffered channel 에서는 하나의 수신자가 데이타를 받을 때까지 송신자가 데이터를 보내는 채널에 묶여 있게 된다. 하지만 Buffered Channel을 사용하면 수신자가 받을 준비가 되어 있지 않아도 지정된 버퍼만큼 데이터를 보내고 다른 일을 수행할 수 있다.

버퍼 채널을 사용하지 않는 경우 수신자 고루틴이 없으면 에러가 발생하는데
버퍼채널을 사용하면 수신자가 당장 없더라도 에러가 발생하지 않는다.

## 채널 파라미터
채널을 함수의 파라미터로 전달할 때, 일반적으로 송수신을 모두 하는 채널이지만
특별히 해당 채널로 송신만 할지 혹은 수신만 할지 지정할 수 있다.

송신파라미터는 (p chan<- int) 를 사용하고, 수신 파라미터는 (p <-chan int) 를 사용한다.

## 채널 닫기
채널을 오픈한 후, close 함수를 사용하여 채널을 닫을 수 있다.
채널을 닫게 되면, 해당 채널로 더이상 송신을 할 수 없지만, 수신은 가능하다.

채널 수신에 사용되는 <- ch 는 두개의 리턴값을 갖는데, 첫째는 채널 메시지이고, 두번째는 수신이 제대로 되었는가를 나타낸다. 만약 채널이 닫혔다면, 두번째 리턴값은 false 를 리턴한다.
```go
package main
 
func main() {
    ch := make(chan int, 2)
     
    // 채널에 송신
    ch <- 1
    ch <- 2
     
    // 채널을 닫는다
    close(ch)
 
    // 채널 수신
    println(<-ch)
    println(<-ch)
     
    if _, success := <-ch; !success {
        println("더이상 데이타 없음.")
    }
}
```

## 채널 range 문
수신자는 for range 문을 사용해 채널이 닫히는 것을 체크할 수 있다.
```go
for i := range ch {
    println(i)
}
```

## 채널 select 문
복수 채널들을 기다리면서 준비된 채널을 실행하는 기능을 한다.
```go
package main

import "time"

func main() {
    done1 := make(chan bool)
    done2 := make(chan bool)

    go run1(done1)
    go run2(done2)

EXIT:
    for {
        select {
        case <-done1:
            println("run1 완료")

        case <-done2:
            println("run2 완료")
            break EXIT
        }
    }
}

func run1(done chan bool) {
    time.Sleep(1 * time.Second)
    done <- true
}

func run2(done chan bool) {
    time.Sleep(2 * time.Second)
    done <- true
}
```

# casting (convert interface)
type 이 interface{} 인 경우, 다음과 같이 casting 할 수 있다.
```go
var t interface{}
s, ok := t.(string)
```


# Q
Assertion,,
Error Type 별 처리
defer, panic, recover 는 유용할 듯
wait 사용법
채널


## 참고
[예제로 배우는 go 프로그래밍](http://golang.site/go/basics)
