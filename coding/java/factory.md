

# fatory method name
- valueOf—Returns an instance that has, loosely speaking, the same value as its parameters. Such static factories are effectively type-conversion methods.
- of—A concise alternative to valueOf, popularized by EnumSet (Item 32).
- getInstance—Returns an instance that is described by the parameters but cannot be said to have the same value. In the case of a singleton, getInstance takes no parameters and returns the sole instance.
- newInstance—Like getInstance, except that newInstance guarantees that each instance returned is distinct from all others.
- getType—Like getInstance, but used when the factory method is in a different class. Type indicates the type of object returned by the factory method.
- newType—Like newInstance, but used when the factory method is in a different class. Type indicates the type of object returned by the factory method.
[Creating and Destroying Java Objects](http://www.informit.com/articles/article.aspx?p=1216151)
