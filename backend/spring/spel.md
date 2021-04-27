# SpEL
Spring Expression Language 의 줄임말, 스프링 EL 이라고 부른다.

## SpEL 표기법
```
#{ SpEL표현식 }
```

## @Value 에서 SpEL 사용
```
@Value("#{1+1}")
int value;
 
@Value("#{'hello ' + 'world'}")
String greeting;
 
@Value("#{1 eq 5}")
boolean trueOrFalse;
 
@Value("Literal String")
String literalString;
 
@Override
public void run(ApplicationArguments args) throws Exception {
    System.out.println(value);
    System.out.println(greeting);
    System.out.println(trueOrFalse);
    System.out.println(literalString);
}
```

## SpEL 과 프로퍼티
```
// application.properties
my.value=100

@Value("#{'${my.value}' eq '100'}")
boolean isEqual;
 
@Override
public void run(ApplicationArguments args) throws Exception {
    System.out.println(isEqual);
}
```

## Bean Reference
`#{beanid.property}` 형식으로 참조
```
import org.springframework.stereotype.Component;
 
@Component
public class Sample {
    
    private int value = 123;
 
    public int getValue() {
        return value;
    }
}


@Value("#{sample.Value}")
int sampleValue;
 
@Override
public void run(ApplicationArguments args) throws Exception {
    System.out.println(sampleValue);
}
```