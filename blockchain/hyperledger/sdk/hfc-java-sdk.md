
# error
orderer 에 sendtransaction 호출할 때, 
> ssl no subject alternative DNS name matching localhost found.
이런 에러가 발생하면 grpcOptions 에 `hostnameOverride` 를 해줘야한다.

servicediscovery 속성 name = `discover`


# ref
[basic tutorial](https://medium.com/@lkolisko/hyperledger-fabric-sdk-java-basics-tutorial-a67b2b898410)