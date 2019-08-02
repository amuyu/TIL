이 tutorial 은 fabric-samples 의 balance-transfer 를 기준으로 작성되었습니다.

# 개발 환경 구성
[Building Your First Network](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)
[Channel Configuration](https://hyperledger-fabric.readthedocs.io/en/latest/configtx.html) : configtx
cryptographic : cryptogen, 
configuration transaction : configtxgen, genesis.block 등 네트워크 구성

# channel 생성
- run the configtxgen tool to generate a genesis block
- run the configtxgen tool to generate an initial binary configuration definition
- get a sign-able channel definition in one of two ways
    1. use the initial binary channel configuration definition
        - use the fabric-client SDK to extract the sign-able channel definition from the initial binary channel configuation definition
    2. build a custom definition
        - use the configtxlator to convert the initial binary channel configuration definition to readable text
        - edit the readable text
        - use the configtalator to convert the edited text to a sign-able channel definition
- use the fabric-client SDK to sign the sign-able channel definition
- use the fabric-client SDK to send the signatures and the sign-able channel definition to the orderer
- use the fabric-client SDK to have the peer join the channel then new channel may be used

서명 가능한 binary channel 초기화는 configtxgen tool 을 사용한다.
configtxget tool은 `configtx.yaml` 의 profile 을 읽어 생성한다.

fabric-client-sdk로 `channel.tx` 의 config 업데이트 요소를 추출한다.
```js
// first read in the file, this gives us a binary config envelope.
let envelope_bytes = fs.readFileSync(path.join(__dirname,'fabric-samples/balance-transfer/artifacts/channel/mychannel.tx'));
// have the nodeSDK extract out the config update
var config_update = client.extractChannelConfig(envelope_bytes);
```
binary config_update 는 서명 프로세스에서 사용될 수 있고, 채널 생성을 위해 orderer 에게 전송할 수 있습니다.

# custom channel 생성
`configtxlator` 가 사람이 읽을 수 있는 JSON 으로 새 채널을 작성하거나 기존 binary 파일을 변환하는 방법이 있다.

`configtxgen` 도구를 사용하여 새로운 채널에 대한 binary 설정을 생성하고, 
Then by sending that binary to the `configtxlator` to convert it to JSON
That JSON could also be used as a template for creating other new channels on your network.

Use the configtxgen tool to produce the binary config files.
```sh
// fabric-samples/balance-transfer/artifacts/channel.
export FABRIC_CFG_PATH = $PWD
../../../bin/configtxgen -outputBlock genesis.block -profile TwoOrgsOrdererGenesis
../../../bin/configtxgen -channelID mychannel -outputCreateChannelTx mychannel.tx -profile TwoOrgsChannel
```
Send the two binary files to the configtxlator service. Since this step is done only once and does not require a Node.js application.
We will use cURL to simplify and speed up getting the results.
First start the configtxlator service, from the `fabric-samples/bin` directory.
```sh
./configtxlator start

curl -X POST --data-binary @genesis.block http://127.0.0.1:7059/protolator/decode/common.Block > genesis.json
curl -X POST --data-binary @mychannel.tx http://127.0.0.1:7059/protolator/decode/common.Envelope > mychannel.json
```
The results of decoding the file `mychannel.tx` which is a common.Envelope contains a common.ConfigUpdate object.
The common.ConfigUpdate is the object that will be signed by all organizations and submitted to the orderer to create a new channel.

```json
{
  "channel_id": "mychannel",
  "read_set": {
    "groups": {
      "Application": {
        "groups": {
          "Org1MSP": {}
        }
      }
    },
    "values": {
      "Consortium": {
        "value": {
          "name": "SampleConsortium"
        }
      }
    }
  },
  "write_set": {
    "groups": {
      "Application": {
        "groups": {
          "Org1MSP": {}
        },
        "mod_policy": "Admins",
        "policies": {
          "Admins": {
            "policy": {
              "type": 3,
              "value": {
                "rule": "MAJORITY",
                "sub_policy": "Admins"
              }
            }
          },
          "Readers": {
            "policy": {
              "type": 3,
              "value": {
                "sub_policy": "Readers"
              }
            }
          },
          "Writers": {
            "policy": {
              "type": 3,
              "value": {
                "sub_policy": "Writers"
              }
            }
          }
        },
        "version": "1"
      }
    },
    "values": {
      "Consortium": {
        "value": {
          "name": "SampleConsortium"
        }
      }
    }
  }
}
```
Note that the Consortium name used must exist on the system channel.
All organizations that you wish to add to the new channel must be defined under in the Consortium section 
with that name on the system channel.
Use the decoded genesis block to verify all values, for example by looking in the
`genesis.json` file generated above. To add an organizations to the channel, they must be placed under the groups section
under the Applications sections as shown above.

Once you have a JSON configuration representing your channel, send it the `configtxlator` to be encoded into a configuration binary.
The following example of sending a REST request to the `configtxlator` uses the Node.js pacakge `superagent` because of the ease
of use for HTTP requests.
```js
var response = superagent.post('http://127.0.0.1:7059/protolator/encode/common.ConfigUpdate',
  config_json.toString())
  .buffer()
  .end((err, res) => {
    if(err) {
      logger.error(err);
      return;
    }
    config_proto = res.body;
  });
```

# Signing and submitting the channel update
The binary configuration must be signed by all organizations.
The application will have to store the binary configuration and have it available to be signed.
Then once the signing is complete, the application will send the binary configuration and all the sigantures to the
orderer using the fabric-client SDK API `createChannel()`. 
```js
var signature = client.signChannelConfig(config_proto);
signatures.push(signature);
```

Now it is time for the channel create.
Note that the orderer must be started with the genesis.block that was generated from the same configuration file as the initial
binary channel configuration definition.

```js
// create an orderer object to represent the orderer of the network
var orderer = client.newOrderer(url,opts);

// have the SDK generate a transaction id
let tx_id = client.newTransactionID();

request = {
  config: config_proto, //the binary config
  signatures : signatures, // the collected signatures
  name : 'mychannel', // the channel name
  orderer : orderer, //the orderer from above
  txId  : tx_id //the generated transaction id
};

// this call will return a Promise
client.createChannel(request)
```

The `createChannel` API returns a Promise to return the status of the submit.
The channel create will take place asynchronously by the orderer.

```js
// set the channel up with network endpoints
var orderer  = client.newOrderer(orderer_url,orderer_opts);
channel.addOrderer(orderer);
var peer = client.newPeer(peer_url,peer_opts);
channel.addPeer(peer);

tx_id = client.newTransactionID();
let g_request = {
  txId :     tx_id
};

// get the genesis block from the orderer
channel.getGenesisBlock(g_request).then((block) =>{
  genesis_block = block;
  tx_id = client.newTransactionID();
  let j_request = {
    targets : targets,
    block : genesis_block,
    txId :     tx_id
  };

  // send genesis block to the peer
  return channel.joinChannel(j_request);
}).then((results) =>{
  if(results && results.response && results.response.status == 200) {
    // join successful
  } else {
    // not good
  }
});
```




# loadConnectionProfile



## loadFromConfig
setAdmin
setMspid
addConnectionOptions

# tip
protobuf ? `common.Envelope` 요소를 포함하는 바이너리 파일
`common.ConfigUpdate` 의 요소가 있음

# 참고
[github.io](https://fabric-sdk-node.github.io/tutorial-network-config.html)

