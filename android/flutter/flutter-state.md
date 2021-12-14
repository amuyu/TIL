# ChangeNotifier 를 사용

## 생성
```
Provider(create: (context) => MyModel())
```

## 조회
```
context.watch<Model>()
context.read<Model>()
```