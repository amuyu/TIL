# let_apply_run_with_
let 도 apply 처럼 사용이 가능하다
let은 객체를 사용하여 다른 메서드를 실행하거나 연산을 수행할 때 사용하기 편하고
apply는 객체의 메서드 및 속성에 접근해서 값을 변경하는데 쓰기 편하다
run 은 let과 apply 은 혼합형처럼 보인다.
apply()는 새로운 객체를 생성하고 객체에 대한 연속된 작업 수행 후, 해당 객체를 사용할 때 사용하고
run()은 해당 객체를 사용할 필요가 없거나 연속된 작업에 대한 결과가 필요할 때 사용하면 될 듯 하다.
╔══════════╦═════════════════╦═══════════════╦═══════════════╗
║ Function ║ Receiver (this) ║ Argument (it) ║    Result     ║
╠══════════╬═════════════════╬═══════════════╬═══════════════╣
║ let      ║ this@MyClass    ║ String("...") ║ Int(42)       ║
║ run      ║ String("...")   ║ N\A           ║ Int(42)       ║
║ run*     ║ this@MyClass    ║ N\A           ║ Int(42)       ║
║ with*    ║ String("...")   ║ N\A           ║ Int(42)       ║
║ apply    ║ String("...")   ║ N\A           ║ String("...") ║
║ also     ║ this@MyClass    ║ String("...") ║ String("...") ║
╚══════════╩═════════════════╩═══════════════╩═══════════════╝

## 참고
[코틀린의 유용한 함수들 - let, apply, run, with](http://kunny.github.io/lecture/kotlin/2016/07/06/kotlin_let_apply_run_with/)
[The difference between Kotlin’s functions: ‘let’, ‘apply’, ‘with’, ‘run’ and ‘also’](https://medium.com/@tpolansk/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8)
