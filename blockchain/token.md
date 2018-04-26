Contract 를 사용해 만들고, Token contract 에 모든 balance 를 가지고 있는다.

### erc223
새로운 token 에 대한 이벤트도 전달
erc20 의 경우 eth 에 대해서만 이벤트 전달

# token transfer
Transfer Event 를 통해 받을 수 있음...

# token receipt service
approve > contract.call() > transferFrom
approve() , approveAndCall(), transferAndCall()
[Ethereum smart service payment with tokens](https://medium.com/@jgm.orinoco/ethereum-smart-service-payment-with-tokens-60894a79f75c)

# 참고
[Create your own crypto-currency](https://ethereum.org/token)
[BUILDING ROBUST SMART CONTRACTS WITH OPENZEPPELIN](http://truffleframework.com/tutorials/robust-smart-contracts-with-openzeppelin)
[erc223-ContractReceiver](https://ethereum.stackexchange.com/a/23623)
