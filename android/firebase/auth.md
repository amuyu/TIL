# Firebase 인증

## Emali Sign In
Email Auth 를 사용해서 로그인 하는 경우, FirebaseAuth 의 signInWithEmailAndPassword 를 호출한다.
인증에 성공하면 Task<AuthResult> 에서 getRsult().getUser() 로 사용자 정보를 가져올 수 있다.
```java
FirebaseAuth.getInstance().signInWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
            @Override
            public void onComplete(@NonNull Task<AuthResult> task) {
                if (task.isSuccessful()) {
                    FirebaseUser user = task.getResult().getUser();
                } else {
                    // fail
                }
            }
        });
```


## 사용자 ID 호출
```java
FirebaseAuth.getInstance().getCurrentUser().getUid();
```

## 참고
[Firebase 인증](https://firebase.google.com/docs/auth/)
[Firebase Auth 와 함께 구글/페이스북 로그인](https://medium.com/askdjango-android/firebase-auth-와-함께-구글-페이스북-로그인-6ce65c124e5)
