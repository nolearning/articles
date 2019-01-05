# instanceof and constructor

## instanceof
Checking Whether an Object Is an Instance of a Given Constructor
```JavaScript
{} instanceof Object // ==> true
[] instanceof Array && [] instanceof Object // ==> true
```

## constructor
```Javascript
function C() {}
C.prototype.constructor == C // each functoin has a instance property 
let obj = new C();
obj.constructor === C
obj.constructor.name === 'C'
new obj.constructor === C

// Avoid
C.prototype = {
    method1: function (...) { ... },
    ...
};

// Prefer:
C.prototype.method1 = function (...) { ... };
```

## Symbol.hasInstance
```JavaScript
class PrimitiveNumber {
    static [Symbol.hasInstance](x) {
        return typeof x === 'number';
    }
}
console.log(123 instanceof PrimitiveNumber); // true

```

## References
* [Speaking JavaScript Chapter 9. Operators](http://speakingjs.com/es5/ch09.html#isobject_typeof)
* [Speaking JavaScript Chapter 17. Objects and Inheritance](http://speakingjs.com/es5/ch17.html#constructor_property)
* [What is the difference between typeof and instanceof and when should one be used vs. the other?](https://stackoverflow.com/questions/899574/what-is-the-difference-between-typeof-and-instanceof-and-when-should-one-be-used)
* [Beyond typeof and instanceof: simplifying dynamic type checks](http://2ality.com/2017/08/type-right.html)

