



```shell
aws ses create-template --cli-input-json file://tt.json
```
금지 목록 조회
```shell
aws sesv2 list-suppressed-destinations
aws sesv2 list-suppressed-destinations --next-token=$TOKEN > mail$INDEX.json
```


[ses sample code](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/ses)