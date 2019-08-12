# Protocol istanbul/64 failed and Genesis block mismatch
```sh
sudo kend stop
sudo rm -rf /var/kend/data
sudo ken init --datadir /var/kend/data genesis.json
sudo kend start
```


# ref
[trobleshooting](https://docs.klaytn.com/node/troubleshooting)