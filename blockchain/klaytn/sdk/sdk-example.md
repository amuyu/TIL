
# abi
contract를 호출하려면 abi가 있어야 한다?
https://medium.com/klaytn/caver-java-dynamic-abi-loader-8ccef10e7e68
caver-java 1.5.1 부터는 abi파일과 binary data를 동적으로 로딩하여 사용할 수 있도록 dynamic abi loader 기능이 추가되었다.

## 배포된 Contract 연결하기
```java
String abi = "Contract ABI Data";
String contractAddress = "0xbe3867496bc619dff5467ceeb8d72779927dbe7a";
Contract contract = caver.contract.create(abi, contractAddress);
System.out.println(contract.getContractAddress());
```

# call
con



# ref
[caver-java-example](https://github.com/klaytn/caver-java-examples)