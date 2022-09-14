
```
@Test
public void testBatchRequest() throws Exception {
    BatchResponse batchResponse = caver.getRpc()
                                       .newBatch()
                                       .add(caver.getRpc().klay.getBlockNumber())
                                       .add(caver.getRpc().net.getPeerCount())
                                       .send();
    Quantity blockNumber = (Quantity) batchResponse.getResponses().get(0);
    assertTrue(blockNumber.getValue().intValue() >= 0);
    Quantity klayPeerCount = (Quantity) batchResponse.getResponses().get(1);
    assertTrue(klayPeerCount.getValue().intValue() >= 0);
}
```

https://forum.klaytn.foundation/t/web3j-batchrequest-caver-java/5120