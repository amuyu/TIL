

# docer exec 실행 후, 결과 받기
docker 라는게 중점은 아니지만 command 실행 후, 결과를 변수에 넣어 실행하고 싶다.
```sh
docker exec -it depositbot-test-tbears tbears deploy ./score/did.zip -c ./score/deploy-did.json 
```
위 명령의 실행 결과는 다음과 같다. 여기서 hash 정보만 받아서 변수에 셋팅하기 위해서 다음과 같이 실행한다.
```
Send deploy request successfully.
If you want to check SCORE deployed successfully, execute txresult command
transaction hash: 0x63e6563b7762325463b1a87407e3433634138ce2eb4d99f180c21e3373123a3d
```
sed 명령을 사용해서 문자열을 파싱한다. 
```
TX_HASH=$(docker exec -it depositbot-test-tbears tbears deploy ./score/did.zip -c ./score/deploy-did.json | sed -n 's/.*\(0x[a-z|0-9]\{64\}\).*/\1/p')
```


# if 옵션
-z : 문자열의 길이가 0이면 참
```
if [ -z $A ];
```

-n : 문자열의 길이가 0이면 아니면 참
```
if [ -n $A ];
```