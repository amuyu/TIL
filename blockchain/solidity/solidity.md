# solidity 사용
## 블록과 거래 속성들
블록과 거래 속성을 가져올 수 있는 변수 및 함수
msg.sender, msg.value, now


# transfer/send/call 작동 비교
transfer : 함수를 호출하는 주소에서 지정된 주소로 이더를 송금. 실패하거나, 2300가스를 초과하면 예외 발생
send : 함수를 호출하는 주소에서 지정된 주소로 이더를 송금. 실패하면 false, 2300가스를 초과하면 예외 발생
call.vaule(1).gas(10) : EVM의 call 호출. value에 송금할 이더, gas에 gas limit. 예외가 발생하면 false

# 함수의 가시성에 따른 내외부 함수 호출
external : this.a()
public : a(), this.a()
internal : a()
private : a()

## string to bytes32
https://ethereum.stackexchange.com/a/9152
function stringToBytes32(string memory source) returns (bytes32 result) {
    bytes memory tempEmptyStringTest = bytes(source);
    if (tempEmptyStringTest.length == 0) {
        return 0x0;
    }

    assembly {
        result := mload(add(source, 32))
    }
}

## bytes32 to string
https://ethereum.stackexchange.com/questions/2519/how-to-convert-a-bytes32-to-string

## 추상 컨트랙트
구현되지 않은 함수를 가지고 있는 함수
new 를 이용해서 인스턴스를 생성하지 못한다

## 라이브러리
라이브러리를 호출할 때는 EVM의 DELEGATECALL을 호출,
다른 컨트랙트의 함수를 호출하면 메시지 콜로 전달되기 때문에 실행 콘텍스트가 호출되는 컨트랙트로 변경된다
라이브러리의 함수를 호출하면 실행 콘텍스트가 바뀌지 않는다. (this)
### 라이브러리 제약
- 상태 변수를 정의할 수 없다
- 상속하거나 상속 받을 수 없다
- 이더를 전송받을 수 없다
### using A for B
A의 라이브러리 함수를 B타입에 추가할 수 있다
호출에 사용된 객체는 호출된 함수의 첫 번째 매개변수로 전달

## selfdestruct



[solidity 문서](http://solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages)
[call vs delegatecall vs library](https://vomtom.at/address-call-vs-delegatecall-vs-libraries/)
