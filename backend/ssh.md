
# sshpass 설치 
```
brew install hudochenkov/sshpass/sshpass
```

# tunneling
기본 명령어
```
ssh -i {key.pem} -L {local-port}:{remote-ip}:{remote-port} {user}@{gateway-ip}
```


Local -> G -> R
Run scp to machine R, which is only accessible through gateway machine G
Step 1: Establish SSH tunnel. Pick a temporary port between 1024 and 32768 (1234 in this example). Port 22 will be used by scp.
```shell
$ ssh -L 1234:<address of R known to G>:22 <user at G>@<address of G> 
```

Step 2: Run scp against port 1234 pretending 127.0.0.1 (localhost) is the remote machine R, and the command will be sent to R.
```shell
$ scp -P 1234 <user at R>@127.0.0.1:/path/to/file file-name-to-be-copied
```

# ref
[Running SCP Through SSH Tunnel](https://www.urbaninsight.com/article/running-scp-through-ssh-tunnel)
[aws ec2 tunneling](https://m.blog.naver.com/xzeed/221746702584)