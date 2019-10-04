

# Housekeeping
가장 기본 적인 chaincode 형태는 다음과 같다.
`shim` api 를 사용하고 기본적으로 `Init` 과 `Invoke` 를 구현해야 한다.
```go
package main

import (
    "fmt"

    "github.com/hyperledger/fabric/core/chaincode/shim"
    "github.com/hyperledger/fabric/protos/peer"
)

// SimpleAsset implements a simple chaincode to manage an asset
type SimpleAsset struct {
}

// Init is called during chaincode instantiation to initialize any
// data. Note that chaincode upgrade also calls this function to reset
// or to migrate data.
func (t *SimpleAsset) Init(stub shim.ChaincodeStubInterface) peer.Response {
    // Get the args from the transaction proposal
    args := stub.GetStringArgs()
    if len(args) != 2 {
            return shim.Error("Incorrect arguments. Expecting a key and a value")
    }

    // Set up any variables or assets here by calling stub.PutState()

    // We store the key and the value on the ledger
    err := stub.PutState(args[0], []byte(args[1]))
    if err != nil {
            return shim.Error(fmt.Sprintf("Failed to create asset: %s", args[0]))
    }
    return shim.Success(nil)
}

// Invoke is called per transaction on the chaincode. Each transaction is
// either a 'get' or a 'set' on the asset created by Init function. The Set
// method may create a new asset by specifying a new key-value pair.
func (t *SimpleAsset) Invoke(stub shim.ChaincodeStubInterface) peer.Response {
    // Extract the function and args from the transaction proposal
    fn, args := stub.GetFunctionAndParameters()

    var result string
    var err error
    if fn == "set" {
            result, err = set(stub, args)
    } else { // assume 'get' even if fn is nil
            result, err = get(stub, args)
    }
    if err != nil {
            return shim.Error(err.Error())
    }

    // Return the result as success payload
    return shim.Success([]byte(result))
}

// Set stores the asset (both key and value) on the ledger. If the key exists,
// it will override the value with the new one
func set(stub shim.ChaincodeStubInterface, args []string) (string, error) {
    if len(args) != 2 {
            return "", fmt.Errorf("Incorrect arguments. Expecting a key and a value")
    }

    err := stub.PutState(args[0], []byte(args[1]))
    if err != nil {
            return "", fmt.Errorf("Failed to set asset: %s", args[0])
    }
    return args[1], nil
}

// Get returns the value of the specified asset key
func get(stub shim.ChaincodeStubInterface, args []string) (string, error) {
    if len(args) != 1 {
            return "", fmt.Errorf("Incorrect arguments. Expecting a key")
    }

    value, err := stub.GetState(args[0])
    if err != nil {
            return "", fmt.Errorf("Failed to get asset: %s with error: %s", args[0], err)
    }
    if value == nil {
            return "", fmt.Errorf("Asset not found: %s", args[0])
    }
    return string(value), nil
}

// main function starts up the chaincode in the container during instantiate
func main() {
    if err := shim.Start(new(SimpleAsset)); err != nil {
            fmt.Printf("Error starting SimpleAsset chaincode: %s", err)
    }
}
```

# Testing Using dev mode
chaincode 는 peer 가 시작하고 유지관리한다. `dev 모드` 에서는 사용자가 체인코드를 작성하고 시작한다.
이 모드는 빠른 코드/빌드/실행/디버그에 유용하다.

## Start the network
fabric-sample 에 `chaincode-docker-devmode` 가 있는데 이를 사용해서 dev 모드로 네트워크를 시작할 수 있다.
```sh
docker-compose -f docker-compose-simple.yaml up
```

## Build & start the chaincode
chaincode bash 에 접근한다.
```bash
docker exec -it chaincode bash
```
그러면 다음과 같은 경로로 shell 이 뜨는데, sacc 로 이동 후, `go build` 를 호출하면 chaincode 가 빌드되고
```bash
root@d2629980e76b:/opt/gopath/src/chaincode#
cd sacc
go build
```
다음 명령으로 chaincode 를 시작할 수 있다.
```bash
CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=mycc:0 ./sacc
```
이 단계에서 체인코드는 어떤 채널과도 연결되어 있지 않기 때문에 `instantiate` 명령을 사용해서 연결해야 한다.

## Use the chaincode
`--peer-chaincodedev` 모드에 있더라도 체인코드를 설치해야 한다.
CLI 컨테이너를 활용하여 다음과 같이 호출한다.
```bash
docker exec -it cli bash
peer chaincode install -p chaincodedev/chaincode/sacc -n mycc -v 0
peer chaincode instantiate -n mycc -v 0 -c '{"Args":["a","10"]}' -C myc
```
이제 호출을 해서 "a" 의 값을 20 으로 변경한다.
```bash
peer chaincode invoke -n mycc -c '{"Args":["set", "a", "20"]}' -C myc
peer chaincode query -n mycc -c '{"Args":["query","a"]}' -C myc
```

## Testing new chaincode
docker 에서 mount 하는 chaincode path 의 subdirectory 에 chaincode 를 넣고 네트워크를 새로 시작하면 된다.

## Chaincode access control
Chaincode 에서는 `GetCreator()` function 을 호출해서 client 인증서를 활용할 수 있다.
Go shim 은 인증서에서 클라이언트 ID, 조직 ID, 클라이언트 ID 속성을 기반으로 access 제어를 할 수 있는 확장 API 를 제공한다.

자세한 내용은 다음을 참고한다. [cid](https://github.com/hyperledger/fabric/tree/release-1.4/core/chaincode/lib/cid)


## Managing external dependencies for chaincode
체인코드에 Go 표준 라이브러리에서 제공하지 않는 패키지가 필요한 경우 해당 패키지를 체인코드와 함께 포함해야 한다.
```bash
govendor init
govendor add +external
govendor add github.com/external/pkg
govendor add github.com/external/pkg/...
```


# ref
[chaincode tutorial](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode.html)
[go chaincode api](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#Chaincode)