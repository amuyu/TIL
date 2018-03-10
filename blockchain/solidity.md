# solidity 사용
## 블록과 거래 속성들
블록과 거래 속성을 가져올 수 있는 변수 및 함수
msg.sender, msg.value, now


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



[solidity 문서](http://solidity.readthedocs.io/en/develop/installing-solidity.html#binary-packages)
