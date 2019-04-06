## groovy
스크립트 언어
동적 타이핑 언어

### closure

정의 후, 나중에 실행 할 코드 조각

함수를 영역 안에 묶어버리는 추상적인 묶음

그루비 클로져는 코드 블록 혹은 메서드 포인터와 같다.

클로져의 파라미터는 `->` 토큰 앞에 나열한다.

인자 하나만을 받는다면 파라미터에서 빼고 `it` 으로 사용한다.

메서드에서 클로저를 마지막 인자로 받는다면 아래와 같이 클로저를 정의할 수 있다.

```
def list = ['a','b','c','d']
def newList = []
list.collect( newList ) {
	it.toUpperCase()
}
println newList           
//  ["A", "B", "C", "D"]
```

collect 메소드는 List와 클로저 인자를 받아들인다.

#### closure 만들기

```groovy
List.metaClass.print = { closure
	for (obj in delegate){
		closure(obj)
	}
}

[1.2.3].print {
    print it
}
```


# 문법
클로저
```groovy
def cl={param1, param2 -> param1+param2}
def cl_mul={param, param2 -> param2*param2}

def func_cl(pc){
        def val=1
        pc(val,2)
}

println "${func_cl(cl)}"
println "${func_cl(cl_mul)}"

// 출처: http://uniksy1106.tistory.com/53 [* 루이지노의 행복한 이야기 : )]
```
collect
```groovy
[1,2,3].collect{it=it*it}
```

[apache groovy](http://groovy-lang.org/documentation.html#gettingstarted)
[Groovy Tutorial](https://www.tutorialspoint.com/groovy/index.htm)
[fufunstudy](https://github.com/funfunStudy/study/wiki/Groovy)
