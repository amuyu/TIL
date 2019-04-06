### java-token-test
IRC2 Token and CrowdSale Test using Java SDK

### Repo
[gitlab](https://repo.theloop.co.kr/icon/java-token-test.git)

### params count
parameter 개수는 적게
```java
private Bytes deployScore(Wallet fromWallet, String zipfile, RpcObject params) throws IOException {
  ...
}
```

### try catch & while condition
exception 났을 때 재조회 코드로 catch 안에서 sleep 호출
result 조회가 되면 while 빠져나오면 되므로 (result == null) 사용
```java
private TransactionResult getTransactionResult(Bytes txHash) throws IOException {
        TransactionResult result = null;
        while (result == null) {
            try {
                result = iconService.getTransactionResult(txHash).execute();
            } catch (RpcError e) {
                System.out.println("RpcError: code: " + e.getCode() + ", message: " + e.getMessage());
                try {
                    // wait until block confirmation
                    System.out.println("Sleep 1 second.");
                    Thread.sleep(1000);
                } catch (InterruptedException ie) {
                    ie.printStackTrace();
                }
            }
        }
        return result;
    }
```

### exception 처리
library 에서 발생하는 exception 은 try-catch 로 감싸고
message 와 함께 Exception 정리
```java
private static KeyWallet createAndStoreWallet() throws IOException {
        try {
            KeyWallet wallet = KeyWallet.create();
            KeyWallet.store(wallet, "P@sswOrd", new File("/tmp"));
            return wallet;
        } catch (InvalidAlgorithmParameterException | NoSuchAlgorithmException | NoSuchProviderException e) {
            e.printStackTrace();
            throw new IOException("Key creation failed!");
        } catch (CipherException e) {
            e.printStackTrace();
            throw new IOException("Key store failed!");
        }
    }
```

### path 사용
Paths 사용으로 file 객체 사용시 path 호출
```java
String zipfile
Path path = Paths.get(zipfile);
return Files.readAllBytes(path);
```
