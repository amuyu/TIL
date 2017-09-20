# single
```kotlin
Single.create(SingleOnSubscribe<String> { e -> e.onSuccess("") })
                .observeOn(Schedulers.io())
                .subscribe(Consumer { needless ->
                    Logger.d("single")
                })
```


# Maybe
