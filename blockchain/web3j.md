
# call contract method
Calling constant methods
Constant methods are those that read a value in a smart contract, and do not alter the state of the smart contract. These methods are available with the same method signature as the smart contract they were generated from:
Type result = contract.someMethod(<param1>, ...).send();

# query contract
Querying the state of a smart contract
This functionality is facilitated by the eth_call JSON-RPC call.

eth_call allows you to call a method on a smart contract to query a value. There is no transaction cost associated with this function, this is because it does not change the state of any smart contract method’s called, it simply returns the value from them:
```java
Function function = new Function<>(
             "functionName",
             Arrays.asList(new Type(value)),  // Solidity Types in smart contract functions
             Arrays.asList(new TypeReference<Type>() {}, ...));

String encodedFunction = FunctionEncoder.encode(function)
org.web3j.protocol.core.methods.response.EthCall response = web3j.ethCall(
             Transaction.createEthCallTransaction(<from>, contractAddress, encodedFunction),
             DefaultBlockParameterName.LATEST)
             .sendAsync().get();

List<Type> someTypes = FunctionReturnDecoder.decode(
             response.getValue(), function.getOutputParameters());
```
Note: If an invalid function call is made, or a null result is obtained, the return value will be an instance of Collections.emptyList()

# ref
[web3j](https://docs.web3j.io/smart_contracts.html#eip)
