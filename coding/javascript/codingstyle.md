Braces are used for all control structures
- No line break before the opening brace.
- Line break after the opening brace.
- Line break before the closing brace.
- Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. Specifically, there is no line break after the brace if it is followed by else, catch, while, or a comma, semicolon, or right-parenthesis
```js
class InnerClass {
  constructor() {}

  /** @param {number} foo */
  method(foo) {
    if (condition(foo)) {
      try {
        // Note: this might fail.
        something();
      } catch (err) {
        recover();
      }
    }
  }
}
```

# Iterators and Generators

# high order function style

# denodeify와 nodeify
denodeify : callback > promise
nodeify : promise > callback

[google](https://google.github.io/styleguide/jsguide.html)
[airbnb](https://github.com/airbnb/javascript)
