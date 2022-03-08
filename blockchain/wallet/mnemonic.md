


```
m / purpose' / coin_type' / account' / change / address_index
```
* m : Mnemonic 의 약자인 것으로 추정된다.
* purpose : BIP44의 44를 뜻한다.
* coin_type : 코인의 번호다. (현재 등록된 코인 번호)
* account : Public Key / Address 의 개념이 아닌 이것들을 담을 수 있는 계정이다. (하나의 계정에 여러개의 Address를 생성할 수 있다.)(EOS의 계정을 사용해보신 분들은 이해가 조금 쉬울 것 같다.) (국민은행 / 신한은행 / 카카오뱅크 정도라고 생각하면 쉽게 이해하시려나..ㅎ)
* change : 0은 내부 체인 1은 외부 체인이라고 한다. 외부 체인은 지갑 외부에서 볼 수 있도록 의도된 주소 (입금용 Address) 내부 체인은 지갑 외부에 표시 되지 않고 반송 Transaction Change에 사용 된다.
* address : n번째 지갑



# ref
[Mnemonic과 HD Wallet](https://ryublock.tistory.com/8)