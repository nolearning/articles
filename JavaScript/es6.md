# ES6

## TYPE

### Symbol
```JavaScript
let symbol1 = Symbol();
let symbol2 = Symbol("label");
let symbol3 = Symbol("label");
let symbol4 = Symbol.for("label");
let symbol5 = Symbol.for("label");

symbol1 != symbol2; // ==> true, not equal to symbol3..5
symbol2 != symbol3; // ==> true 
symbol2 != symbol4; // ==> true
symbol4 == symbol5; // ==> true
```
  symbol is unique intrnal data(mostly is unique string internally), can be used as identity for JS object&class. The original motivation was to enable private properties. One feature is to resolve the name conflict problem in many library. JavaScript symbol may have GC problem, as the symbol mostly cannot be collected as they be used as key for object or class.
  
In Ruby symbol is immutable object, like constant strings, as in ruby, a string is mutable.
```ruby
x = :sym
y = :sym
(x.__id__ == y.__id__ ) && ( :sym.__id__ == x.__id__) # => true

x = "string"
y = "string"
(x.__id__ == y.__id__ ) || ( "string".__id__ == x.__id__) # => false
```

Symbol as object's key.
```JavaScript
let obj = {
    [Symbol('my_key')]: 1,
    enum: 2,
    nonEnum: 3
};
Object.defineProperty(obj,
    'nonEnum', { enumerable: false });
Object.getOwnPropertyNames(obj); // ==> ['enum', 'nonEnum']    
Object.getOwnPropertySymbols(obj)  // ==> [Symbol(my_key)]
Reflect.ownKeys(obj)   // ==> [Symbol(my_key), 'enum', 'nonEnum']
Object.keys(obj)    // ==> ['enum']
```
* [ES6 In Depth: Symbols](https://hacks.mozilla.org/2015/06/es6-in-depth-symbols/)
* [What is the motivation for bringing Symbols to ES6?](https://stackoverflow.com/questions/21724326/what-is-the-motivation-for-bringing-symbols-to-es6)
* [Symbols in ECMAScript 6](http://2ality.com/2014/12/es6-symbols.html)

* [How to understand symbols in Ruby](https://stackoverflow.com/questions/2341837/how-to-understand-symbols-in-ruby)
* [What is a symbol in Ruby?](https://softwareengineering.stackexchange.com/questions/24460/what-is-a-symbol-in-ruby)
