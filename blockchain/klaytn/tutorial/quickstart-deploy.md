# Deploy a Smart Contract

## Creating a project directory
```sh
mkdir klaytn-testboard
$ cd klaytn-testboard
```

## Initializing Truffle
```sh
truffle init
```

## Writing a simple contract
Create KlaytnGreeter.sol at klaytn-testboard/contracts directory.
```sh
$ cd contracts
$ touch KlaytnGreeter.sol
$ vi KlaytnGreeter.sol
````
Write the following code in KlaytnGreeter.sol.
```solidity
pragma solidity 0.4.24;
contract Mortal {
    /* Define variable owner of the type address */
    address owner;
    /* This function is executed at initialization and sets the owner of the contract */
    function Mortal() { owner = msg.sender; }
    /* Function to recover the funds on the contract */
    function kill() { if (msg.sender == owner) selfdestruct(owner); }
}

contract KlaytnGreeter is Mortal {
    /* Define variable greeting of the type string */
    string greeting;
    /* This runs when the contract is executed */
    function KlaytnGreeter(string _greeting) public {
        greeting = _greeting;
    }
    /* Main function */
    function greet() constant returns (string) {
        return greeting;
    }
}
```


## Modifying the Migration Script
Modify 1_initial_migration.js as the following.
```js
const Migrations = artifacts.require("./Migrations.sol");
const KlaytnGreeter = artifacts.require("./KlaytnGreeter.sol");
module.exports = function(deployer) {
  deployer.deploy(Migrations);
  deployer.deploy(KlaytnGreeter, 'Hello, Klaytn');
};
```


## Deploying a smart contract using truffle
truffle-config.js
```js
// truffle-config.js
module.exports = {
    networks: {
        klaytn: {
            host: '127.0.0.1',
            port: 8551,
            from: '0x75a59b94889a05c03c66c3c84e9d2f8308ca4abd', // enter your account address
            network_id: '1001', // Baobab network id
            gas: 20000000, // transaction gas limit
            gasPrice: 25000000000, // gasPrice of Baobab is 25 Gpeb
        },
    },
    compilers: {
      solc: {
        version: "0.4.24"    // Specify compiler's version to 0.4.24
      }
  }
};
```
Deploy the contract using the following command.
```sh
$ truffle deploy --network klaytn --reset
Using network 'klaytn'.
Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x0f5108bd9e51fe6bf71dfc472577e3f55519e0b5d140a99bf65faf26830acfca
  Migrations: 0x97b1b3735c8f2326a262dbbe6c574a8ea1ba0b7d
  Deploying KlaytnGreeter...
  ... 0xcba53b6090cb4a118359b27293ba95116a8f35f66ae50fbd23ae1081ce9ffb9e
  KlaytnGreeter: [SAVE THIS ADDRESS!!] # this is your smart contract address
Saving successful migration to network...
  ... 0x14eb68727ca5a0ac767441c9b7ab077336f9311f71e9854d42c617aebceeec72
Saving artifacts...
```

첫 Delpoy
```
...
Running migration: 1_initial_migration.js
  Deploying KlaytnGreeter...
  ... 0x7a4fea512676af0f1fae0fb014def229a069e6262bfcbbaa1fdd722dad9cb290
  KlaytnGreeter: 0x5b8e67038912c24e533b98d86d9a70e638d354c1
Saving artifacts...
...
```

result
```
Compiling your contracts...
===========================
✔ Fetching solc version list from solc-bin. Attempt #1
✔ Downloading compiler. Attempt #1.
> Everything is up to date, there is nothing to compile.



Starting migrations...
======================
> Network name:    'baobab'
> Network id:      1001
> Block gas limit: 0 (0x0)


1_initial_migration.js
======================

   Deploying 'Migrations'
   ----------------------
   > transaction hash:    0x65d5ac9a528d04e20dbae0a2c18420c6fd3564a68dd090eed44e99deee9dedfd
   > Blocks: 0            Seconds: 0
   > contract address:    0xC96487124C235d3c040e3e6Cf7949A35f1DFE5de
   > block number:        81004185
   > block timestamp:     1642572928
   > account:             0x4b02B152E205C5DBfE86fd3565Eb4cE2AC964C07
   > balance:             14.78782405
   > gas used:            243519 (0x3b73f)
   > gas price:           25 gwei
   > value sent:          0 ETH
   > total cost:          0.006087975 ETH


   Deploying 'KlaytnGreeter'
   -------------------------
   > transaction hash:    0x9fa4718b1bddc601c5b7a2d5a5c23ba709be4ec760b7c9a682f6bd04bd8ea77f
   > Blocks: 0            Seconds: 0
   > contract address:    0x97e90b7A8F8C085f1cB67dE9a5E14b663b6ddFDF
   > block number:        81004188
   > block timestamp:     1642572931
   > account:             0x4b02B152E205C5DBfE86fd3565Eb4cE2AC964C07
   > balance:             14.78004825
   > gas used:            311032 (0x4bef8)
   > gas price:           25 gwei
   > value sent:          0 ETH
   > total cost:          0.0077758 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:         0.013863775 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0.013863775 ETH
```


## ref
[스마트컨트랙트 배포](https://ko.docs.klaytn.com/getting-started/quick-start/deploy-a-smart-contract)