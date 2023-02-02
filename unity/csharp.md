
# contents
- 변수선언
- Delegates
- Events
- Action


Task
function 이름은 대문자?
namesapce
Controller?
Manager?
App?
event delegate

### 변수 선언
`var` : 자료형을 자동으로 저장, 선언과 동시에 초기화, Null 은 안된다?
```
int a;
var a = 100;
```

### nullable
```
int? a = null;
```

### Delegates
함수 포인트
#### callback, 함수 참조

 - Delegate는 대리자라고도 하며, 메서드에 대한 참조를 갖는 형식이다.
 - 함수포인터나 콜백과 동일한 동작으로 delegate를 호출하면 참조하고 있는 메서드가 호출된다.
 - 참조하는 함수의 반환 형식 및 매개변수를 사용하여 선언한다.
  * 선언한 함수 형식이 일치하는 메서드에 대해서만 참조할 수 있다.

```csharp
namespace ConsoleApplication
{
    delegate int FuncDelegate(int a, int b);

    class Program
    {
        static int Plus(int a, int b)
        {
            return a + b;
        }
        
        static int Minus(int a, int b)
        {
            return a - b;
        }

        static void Main(string[] args)
        {
            FuncDelegate plusDelegate = Plus;
            FuncDelegate minusDelegate = Minus;

            Console.WriteLine(plusDelegate(5, 10));
            Console.WriteLine(minusDelegate(20, 10));
        }
    }
}
```
#### 델리게이트 체인
하나의 delegat는 여러개의 함수를 등록하고 호출할 수 있는 델리게이트 체인 지원
```csharp
using System;

namespace ConsoleApp1
{
    delegate void FuncDelegate(string str);

    class Program
    {
        static void Func1(string str)
        {
            Console.WriteLine("Helo1 : " + str);
        }
        static void Func2(string str)
        {
            Console.WriteLine("Helo2 : " + str);
        }
        static void Func3(string str)
        {
            Console.WriteLine("Helo3 : " + str);
        }
        static void Func4(string str)
        {
            Console.WriteLine("Helo4 : " + str);
        }
        static void Main(string[] args)
        {
            FuncDelegate plusDelegate = null;
            plusDelegate += Func1;
            plusDelegate += Func2;
            plusDelegate += Func3;
            plusDelegate += Func4;

            plusDelegate("Text");
        }
    }
}
```

### Events
이벤트는 delegate model을 기반으로 하며, observer design pattern을 따른다.

### Action
값을 반환하지 않는 함수를 캡슐화 하는 대리자 
`Action<T>`

### issue
cating delegate
```csharp
// Two delegate types with the same signature
delegate void Foo();
delegate void Bar();

class Test
{
    static void Main()
    {
        // Actual value is irrelevant here
        Foo foo = null;

        // Error: can't convert like this
        // Bar bar = foo;

        // This works fine
        Bar bar = new Bar(foo);
    }
}
```
https://stackoverflow.com/questions/3465113/casting-delegate

## ref
[coding-conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
[delegate, event 사용법 및 사용예제](https://frozenpond.tistory.com/3)