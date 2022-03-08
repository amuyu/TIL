# Protocol istanbul/64 failed and Genesis block mismatch
```sh
sudo kend stop
sudo rm -rf /var/kend/data
sudo ken init --datadir /var/kend/data genesis.json
sudo kend start
```


# rever message
transaction revert message 확인하는 방법
```
// https://forum.klaytn.com/t/klaytn-ide-transaction-debugging/809/2
debug.traceTransaction("txhash", {tracer: "revertTracer"})
```

# en 동기화가 안될 때 (baobab)
https://forum.klaytn.com/t/archive-node-peer-sync/2668/3
```
# addPeer
admin.addPeer("kni://0412e1fe709512a2ae7387eafba0036c07195a8b63921ce8bd708b1c02db21366efcc690bc50801eb6c74facad42f8d801db289ea3882fe37e56829ad73e8a81@54.180.16.123:32323?ntype=en")
```
```

# ref
[trobleshooting](https://docs.klaytn.com/node/troubleshooting)
[forum](https://forum.klaytn.com/)