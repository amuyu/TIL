

hyperledger 에서 제공하는 `fabric-samples` 에서 [marbles](github.com/hyperledger/fabric-samples/tree/master/chaincode/marbles02) 체인코드를 `fabric-sdk-java` 를 사용해서 호출하는 방법을 정리한다.  전체 코드는 [github](https://github.com/amuyu/fabric-sdk-java-samples) 에서 확인할 수 있다.

## dependency
java 프로젝트에서 `fabric-sdk-java` 를 사용하기 위해 다음과 같이 dependency 설정을 추가한다.
```gradle
dependencies {
    implementation("org.hyperledger.fabric-sdk-java:fabric-sdk-java:1.4.4")
}
```


## 네트워크 시작
먼저, `fabric-sdk-java` 로 호출하기 위한 network 를 시작한다. 네트워크는 `fabric-samples` 를 clone 받아 쉽게 시작할 수 있다. 
여기서는 `fabric-samples` 중에서 [off_chain_data](https://github.com/hyperledger/fabric-samples/tree/master/off_chain_data) 에서 제공하는 스트립트를 사용한다.

`fabric-sample` 를 git clone 받고 `release-1.4*` 를 checkout 받는다. 
그리고 `off_chain_data` 폴더로 이동해서 `startFabric.sh` 를 실행한다.
```sh
./startFabric.sh
```

만약 version 관련 에러가 발생한다면 docker image 에 버전을 명시해줘야 하는데 `startFabric.sh` 를 열어서 네트워크 시작 명령을 다음과 같이 수정한다.
`-i 1.4.2` 옵션을 추가해서 1.4.3 docker image 로 네트워크를 시작할 수 있다.
```
-echo y | ./byfn.sh up -a -n -s couchdb
+echo y | ./byfn.sh up -i 1.4.3 -a -n -s couchdb
```

## 체인코드 확인
`startFabric.sh` 를 실행하면 체인코드 instantiate 까지 진행하는데 체인코드 endorsement 정책을 보면 다음과 같이
`Org1` 과 `Org2` 의 보증이 필요한 것을 확인할 수 있다. 체인코드 실행할 때, 이 정책대로 호출할 수 있도록 코드를 작성해야 한다.
```
    -P "AND('Org1MSP.member','Org2MSP.member')" 
```


## HFCAClient 객체 생성
`fabric-ca-server` 로부터 msp 를 받기 위해 `HFCAClient` 객체를 다음과 같이 셋팅한다.
```java
// https 호출을 위한 설정
String pemFile = "{pemfile 경로}";
Properties properties = new Properties();
properties.put("allowAllHostNames", "true");
properties.put("pemFile", pemFile);

try {
    HFCAClient caClient = HFCAClient.createNewInstance("ca-org1", "https://localhost:7054", properties);
} catch (MalformedURLException | InvalidArgumentException e) {
    throw new FabricCAException(e.getMessage());
}

try {
    caClient.setCryptoSuite(CryptoSuite.Factory.getCryptoSuite());
} catch (IllegalAccessException | InstantiationException | ClassNotFoundException |
        CryptoException | org.hyperledger.fabric.sdk.exception.InvalidArgumentException |
        NoSuchMethodException | InvocationTargetException e) {
    throw new FabricCAException(e.getMessage());
}
```

`pemFile` 경로는 호출하려는 서버에 따라 설정이 달라지는데 
[tls](https://github.com/amuyu/fabric-sdk-java-samples/tree/master/tls) 에서 다운받아 사용하면 된다.

## ca admin 계정 enroll
[hfcaclient 객체 생성](#hfcaclient-객체-생성) 에서 pemFile 을 `ca-org1.pem` 으로 설정해서 생성한 후,
`caClient.enroll` 을 호출하면 `Enrollment` 를 return 받을 수 있다.
```java
// HFCAClient 객체 생성 참고
HFCAClient caClient = ...;
Enrollment e = caClient.enroll("admin", "adminpw");
Registar registar = Registar.builder()
                    .name(enrollment.getName())
                    .enrollment(NEnrollment.of(e))
                    .build();
```
`Registar` 는 `org.hyperledger.fabric.sdk.User` 인터페이스를 갖고 있는 객체로, 사용자 계정을 등록할 때 필요하다.

## 사용자 계정 enroll
`Registar` 객체 생성이 완료 되었으면 체인코드를 호출하기 위한 client 를 등록한다.
```java
// HFCAClient 객체 생성 참고
HFCAClient caClient = ...;
// ca admin 계정 enroll 참고
Registar registar = ...;
RegistrationRequest request = new RegistrationRequest("user1", "org1.department1");
request.addAttribute(new Attribute("role", HFCAClient.HFCA_TYPE_CLIENT));
String secret = caClient.register(request, registar);
Enrollment e =  caClient.enroll(request.getEnrollmentID(), secret);
Client client = Client.builder()
                .name("user1")
                .mspId("Org1MSP")
                .enrollment(NEnrollment.of(e))
                .build();
```
`Client` 는 `Registar` 처럼 `org.hyperledger.fabric.sdk.User` 인터페이스를 갖고 있는 객체로 `FabricService` 를 호출할 때 사용한다.

## HFClient 객체 생성
fabric network(peer/orderer) 에 호출하기 위해서 `HFClient` 를 생성한다.
[사용자 계정 enroll](#사용자-계정-enroll) 에서 생성한 `Client` 객체가 필요하다. 
```java
try {
    // 사용자 계정 enroll 참고
    Client client = ...;
    CryptoSuite cryptoSuite = CryptoSuite.Factory.getCryptoSuite();
    HFClient c = HFClient.createNewInstance();
    c.setCryptoSuite(cryptoSuite);
    c.setUserContext(client);
    return c;
} catch (IllegalAccessException | InstantiationException | ClassNotFoundException |
        CryptoException | org.hyperledger.fabric.sdk.exception.InvalidArgumentException |
        NoSuchMethodException | InvocationTargetException e) {
    throw new FabricServiceException(e.getMessage());
}
```

## 채널 생성 및 초기화
체인코드가 instantiate 된 채널 초기화를 한다. 
채널에 `peer0.org1`, `peer0.org2`, `orderer` 정보를 추가하고 초기화를 한다.
```java
try {
    // HFClient 객체 생성 참고
    HFClient hfClient = ...;
    Channel c = hfClient.newChannel("mychannel");

    // peer0.Org1 추가
    // https://github.com/amuyu/fabric-sdk-java-samples/tree/master/tls
    String peer0Org1PemFile = "{pemFile 경로}";
    Properties p1 = new Properties();
    p1.setProperty("ssl-target-name-override", "peer0.org1.example.com");
    p1.setProperty("hostnameOverride", "peer0.org1.example.com");
    p1.setProperty("pemFile", peer0Org1PemFile);
    c.addPeer(hfClient.newPeer("peer0.org1.example.com", "grpcs://localhost:7051", p1));

    // peer0.Org2 추가
    // https://github.com/amuyu/fabric-sdk-java-samples/tree/master/tls
    String peer0Org2PemFile = "{pemFile 경로}";
    Properties p2 = new Properties();
    p2.setProperty("ssl-target-name-override", "peer0.org2.example.com");
    p2.setProperty("hostnameOverride", "peer0.org2.example.com");
    p2.setProperty("pemFile", peer0Org2PemFile);
    c.addPeer(hfClient.newPeer("peer0.org2.example.com", "grpcs://localhost:9051", p2));

    // orderer 추가
    // https://github.com/amuyu/fabric-sdk-java-samples/tree/master/tls
    String ordererPemFile = "{pemFile 경로}";
    Properties o1 = new Properties();
    o1.setProperty("ssl-target-name-override", "orderer.example.com");
    o1.setProperty("hostnameOverride", "orderer.example.com");
    o1.setProperty("pemFile", ordererPemFile);
    c.addOrderer(hfClient.newOrderer("orderer.example.com", "grpcs://localhost:7050", o1));

    c.initialize();
    logger.info("CHANNEL [{}] INITIALIZED : [{}]", c.getName(), c.isInitialized());

} catch (InvalidArgumentException | TransactionException e) {
    throw new FabricServiceException(e.getMessage());
}
```

## 체인코드 invoke
채널 생성까지 정상적으로 성공하면, 이제 체인코드를 호출 할 수 있다.
먼저, 체인코드의 `initMarble` 를 호출한다.
```java
try {
    // HFClient 객체 생성 참고
    HFClient hfClient = ...;
    // 채널 생성 및 초기화 참고
    Channel channel = ...;
    ChaincodID chaincodeID = ChaincodeID.newBuilder()
            .setName("marbles")
            .setVersion("1.0")
            .build();

    List<String> params = Arrays.asList("marble1","blue","35","tom");
    String fcn = "initMarble";

    // proposal request 객체를 생성한다.
    TransactionProposalRequest request = hfClient.newTransactionProposalRequest();
    request.setChaincodeID(chaincodeID);
    request.setFcn(fcn);
    request.setArgs(new ArrayList<>(args));

    // proposal 에 대한 response 를 확인할 수 있다.
    // proposal 결과에 따라 sendTransaction 을 할지 안할지 판단할 수 있다.
    Collection<ProposalResponse> responses = channel.sendTransactionProposal(request);
    channel.sendTransaction(responses).get();


} catch (InvalidArgumentException | ProposalException | ExecutionException e) {
    throw new FabricServiceException(e.getMessage());
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
    throw new FabricServiceException(e.getMessage());
}
```

## 체인코드 query
위에서 체인코드 invoke 가 정상적으로 호출되었으면, block 이 commit 되고, invoke 한 data를 조회할 수 있다.
체인코드의 `queryMarblesByOwner` 를 호출해서 조회한다.
```java
try {
    // HFClient 객체 생성 참고
    HFClient hfClient = ...;
    // 채널 생성 및 초기화 참고
    Channel channel = ...;
    ChaincodID chaincodeID = ChaincodeID.newBuilder()
                .setName("marbles")
                .setVersion("1.0")
                .build();

    List<String> params =  Collections.singletonList("tom");
    String fcn = "queryMarblesByOwner";

    QueryByChaincodeRequest request = hfClient.newQueryProposalRequest();
    request.setChaincodeID(chaincodeID);
    request.setFcn(fcn);
    request.setArgs(new ArrayList<>(args));
    request.setProposalWaitTime(180000);

    Collection<ProposalResponse> responses = channel.queryByChaincode(request);
    for (ProposalResponse response : responses) {
        logger.info("chaincode propsal - chaincode: name=[{}], version=[{}], status=[{}], message[{}]",
                response.getChaincodeID().getName(), response.getChaincodeID().getVersion(),
                response.getStatus(), response.getMessage());

        boolean success = proposalResponse.getStatus() == ProposalResponse.Status.SUCCESS;
        if (success) {
            try {
                // return 한 데이터를 확인할 수 있다.
                String payload = new String(proposalResponse.getChaincodeActionResponsePayload());
            } catch (InvalidArgumentException e) {
                throw new IllegalArgumentException(e.getMessage());
            }
        }        
    }

} catch (InvalidArgumentException | ProposalException e) {
    throw new FabricServiceException(e.getMessage(), e);
}
```





