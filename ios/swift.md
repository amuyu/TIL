
## The Basics
### 상수와 변수
상수 : let
변수 : var

여러개의 상수 또는 변수를 선언할 때, 콤마로 구분하여 한줄로 선언이 가능합니다.
```swift
var x = 0.0, y = 0.0, z = 0.0
```

타입 명시
```swift
var welcomeMessage: String
```

### 출력
`print(_:separator:terminator:)` 함수로 값을 출력할 수 있습니다.
```swift
print(friendlyWelcome)
```

### String interpolation
`\(var)` 변수(상수)의 값을 문자열에 포함하는 방법
```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
```

### semicolons
세미콜론(;)은 필수조건이 아닙니다. 여러 구문을 한줄로 작성할 경우 세미콜론은 필수로 작성해야 합니다.
```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```

### 정수
8, 16, 32, 64 비트 형태의 정수를 지원합니다. UInt8, Int32,,,,,

### 숫자 리터럴
* 접두사 없는 10진수
* 0b 접두사로 2진수
* 0o 접두사로 8진수
* 0x 접두사로 16진수
```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 in binary notation
let octalInteger = 0o21           // 17 in octal notation
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```

### 정수와 부동 소수점 변환
정수와 부동 소수점 숫자 타입의 변환은 명시적으로 변환해야 합니다.

### 타입 별칭 (Type Aliases)
이미 존재하는 타입을 다른 이름으로 정의합니다.
```swift
typealias AudioSample = UInt16
```

### 튜플 (Tuples)
```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")

let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"

let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"

print("The status code is \(http404Error.0)")
// Prints "The status code is 404"
print("The status message is \(http404Error.1)")
// Prints "The status message is Not Found"

// 튜플에 이름 넣기
let http200Status = (statusCode: 200, description: "OK")
```

### 옵셔널 바인딩
```
if let actualNumber = Int(possibleNumber) {
    print("The string \"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("The string \"\(possibleNumber)\" could not be converted to an integer")
}
// Prints "The string "123" has an integer value of 123"
```

### 에러 처리
```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch SandwichError.outOfCleanDishes {
    washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

### assertions and preconditions
```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 is not >= 0.

// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```

### nil-coalescing operator
`(a ?? b)` 는 옵셔널에 a에 값이 있으면 a, nil 이면 b
```swift
(a ?? b) 
```



some?
protocol?
struct? class와 차이?


### ref
[docs](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)
[번역](https://bbiguduk.gitbook.io/swift/language-guide-1/the-basics)