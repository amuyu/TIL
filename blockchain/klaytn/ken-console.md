
# 콘솔 접속 방법
```
ken attach --datadir ~/kend_home
$ ken attach ~/kend_home/klay.ipc
Welcome to the Klaytn JavaScript console

!instance: Klaytn/vX.X.X/XXXX-XXXX/goX.X.X
 datadir: ~/kend_home
 modules: admin:1.0 debug:1.0 governance:1.0 istanbul:1.0 klay:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0

 >
```

# sendValueTransfer
```
> var tx = {from: "0x4937a6f664630547f6b0c3c235c4f03a64ca36b1", to: "0x4b02b152e205c5dbfe86fd3565eb4ce2ac964c07", value: klay.toPeb(100, "KLAY")}
undefined
> personal.sendValueTransfer(tx, "passphrase")
0x8474441674cdd47b35b875fd1a530b800b51a5264b9975fb21129eeb8c18582f
```