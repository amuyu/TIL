

# response body 가 json 형태가 아닐 때
다음과 같은 에러가 발생한다.
> Use JsonReader.setLenient(true) to accept malformed JSON
gson 을 사용하고 있다면,  

```java
Gson gson = new GsonBuilder()
            .setLenient()
            .create();

Retrofit retrofit = new Retrofit.Builder()
        .addConverterFactory(GsonConverterFactory.create(gson))
        .build();
```

이렇게 셋팅한다.