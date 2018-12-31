# Type Coercion and Gotchas

## Types
* Undefined
* Null
* Boolean
* String
* Number
* Object
* Reference(ecma-262#sec-8)

## typeof
| Type of val | 	Result |
| ---- | --- |
| Undefined 	| "undefined" |
|  Null  |	"object" |
| Boolean |	"boolean" |
|Number |	"number" |
| String  |	"string" |
| Object (native and does not implement [[Call]]) |	"object" |
| Object (native or host and does implement [[Call]]) |	"function" |
| Object (host and does not implement [[Call]])  |	Implementation-defined except may not be "undefined", "boolean", "number", or "string". |

## operator
| operator | evaluation |
| -- | -- |
| ++   -- | SyntaxError or ToNumber |
| +   - (Unary) | ToNumber, -NaN ==> NaN |
| ~  | ToInt32  |
| ! | ToBoolean |
| * / % | left -> right, GetValue, ToNumber |
| + | left -> right, GetValue, ToPrimitive, if(string) ToString else ToNunmber |
| - | left -> right, GetValue, ToNumber |
| == / != / left -> right, GetValue |

```JavaScript
-NaN // ==> NaN
1 * NaN // ==> NaN
0 * Infinity // ==> NaN
1 / Infinity // ==> 0
0 / 0 // ==> NaN
```

## Equality Comparison Algorithm
```JavaScript
  if (Type(x) equal Type(y) {
    if (Type(x) is Undefined or Null) return true;
    if (Type(x) is Number) {
      if(x  isNaN) return false;
      return x == y
    }
    if (Type(x) isString) {
      if (x y hasSameContent) return ture;
      return false;
    }
    if (Type(x) isBoolean) retrn x == y
    if (Type(x) isObject) return isSameObject
    
  } else {
    if (isNull(x) && isUndefined(y)) return true;
    if (isUndefined(x) && isNull(y)) return true;
    if (Type(x) isNumber && Type(y) isString) return x == ToNumber(y)
    if (Type(x) isString && Type(y) isNumber) return ToNumber(x) == y
    if (Type(x) isBoolean) return ToNumber(x) == y
    if (Type(y) isBoolean) return x == ToNumber(y)
    if (Type(x) isNumberOrString && Type(y) isObject) return x == ToPrimitive(y)
    if (Type(x) isObject && Type(y) isNumberOrString ) return ToPrimitive(x) == y
  }
  return false
```

## The Abstract Relational Comparison Algorithm
```

    If the LeftFirst flag is true, then
        Let px be the result of calling ToPrimitive(x, hint Number).
        Let py be the result of calling ToPrimitive(y, hint Number).
    Else the order of evaluation needs to be reversed to preserve left to right evaluation
        Let py be the result of calling ToPrimitive(y, hint Number).
        Let px be the result of calling ToPrimitive(x, hint Number).
    If it is not the case that both Type(px) is String and Type(py) is String, then
        Let nx be the result of calling ToNumber(px). Because px and py are primitive values evaluation order is not important.
        Let ny be the result of calling ToNumber(py).
        If nx is NaN, return undefined.
        If ny is NaN, return undefined.
        If nx and ny are the same Number value, return false.
        If nx is +0 and ny is −0, return false.
        If nx is −0 and ny is +0, return false.
        If nx is +∞, return false.
        If ny is +∞, return true.
        If ny is −∞, return false.
        If nx is −∞, return true.
        If the mathematical value of nx is less than the mathematical value of ny —note that these mathematical values are both finite and not both zero—return true. Otherwise, return false.
    Else, both px and py are Strings
        If py is a prefix of px, return false. (A String value p is a prefix of String value q if q can be the result of concatenating p and some other String r. Note that any String is a prefix of itself, because r may be the empty String.)
        If px is a prefix of py, return true.
        Let k be the smallest nonnegative integer such that the character at position k within px is different from the character at position k within py. (There must be such a k, for neither String is a prefix of the other.)
        Let m be the integer that is the code unit value for the character at position k within px.
        Let n be the integer that is the code unit value for the character at position k within py.
        If m < n, return true. Otherwise, return false.
```
## coercion

* ToPrimitive
  - Object -- [[DefaultValue]] ecma-262#sec-8.12.8
    - hint with string
    ```JavaScript
    if(isCallable(toString))
      return toString
    else if(IsCallable(valueOf))
      val = valueof
      if(isPrimitive(val) return val
    throw TypeError
    ```
    - hint with Number
    ```JavaScript
    if(IsCallable(valueOf))
      val = valueof
      if(isPrimitive(val) return val
    else if(isCallable(toString))
      return toString
    throw TypeError
    ```
* ToBoolean
  - Object -- true
* ToNumber 
  - Undefined -- NaN
  - Null -- 0
  - Object -- ToPrimitive -> ToNumber
  
## Gotchas
```JavaScript
![] == [] // ![] -> !(ToBoolean([]) -> !(ToBoolean(Object) -> !true -> false, [] -> ToPrimitive -> "" , false == "" -> true
{} + [] // {} is parsed as block, +[] -> unary + -> 0
[] + {} // ToPrimitive -> ToString -> [object Object]
+{} // unary + -> ToNumber -> NaN
new String("a") == "a" // Object and string -> ToString -> true
new String("a") == new String("a") // Object and Object -> isSameObject -> false
```

## References
* [ECMAScript® Language Specification - Standard ECMA-262 5.1 Edition](https://www.ecma-international.org/ecma-262/5.1)
* [JavaScript type coercion explained](https://medium.freecodecamp.org/js-type-coercion-explained-27ba3d9a2839)
* [Why does ++[[]][+[]]+[+[]] return the string “10”?](https://stackoverflow.com/questions/7202157/why-does-return-the-string-10)
* [What is {} + {} in JavaScript?](http://2ality.com/2012/01/object-plus-object.html)
