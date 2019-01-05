# Static/Dynamic and Strong/Weak Type

## Static Type vs. Dynamic Type
* Static/Dynamic Typing is about when type information is acquired (Either at compile time or at runtime)
* Static type checking is the process of verifying the type safety of a program based on analysis of a program's text (source code). If a program passes a static type checker, then the program is guaranteed to satisfy some set of type safety properties for all possible inputs. 
* Dynamic type checking is the process of verifying the type safety of a program at runtime. 
* In a statically typed language, every variable name is bound both
  - to a type (at compile time, by means of a data declaration)
  - to an object.  
  - The binding to an object is optional — if a name is not bound to an object, the name is said to be null.
* In a dynamically typed language, every variable name is (unless it is null) bound only to an object.
  - Names are bound to objects at execution time by means of assignment statements, and it is possible to bind a name to objects of different types during the execution of the program.
  - Once a variable name has been bound to a type (that is, declared) it can be bound (via an assignment statement) only to objects of that type; it cannot ever be bound to an object of a different type. An attempt to bind the name to an object of the wrong type will raise a type exception.

## Strong Type vs. Weak Type
* Strong/Weak Typing is about how strictly types are distinguished (e.g. whether the language tries to do an implicit conversion from strings to numbers).
* "Strong typing" generally refers to use of programming language types in order to both capture invariants of the code, and ensure its correctness, and definitely exclude certain classes of programming errors. Thus there are many "strong typing" disciplines used to achieve these goals. 
* Some programming languages make it easy to use a value of one type as if it were a value of another type. This is sometimes described as "weak typing". 
* In a weakly typed language, variables can be implicitly coerced to unrelated types, whereas in a strongly typed language they cannot, and an explicit conversion is required. 

## Reference
* [Type system](https://en.wikipedia.org/wiki/Type_system)
* [Strong and weak typing](https://en.wikipedia.org/wiki/Strong_and_weak_typing)
* [Static/Dynamic vs Strong/Weak](https://stackoverflow.com/questions/2351190/static-dynamic-vs-strong-weak)
* [Static vs. dynamic typing of programming languages](https://pythonconquerstheuniverse.wordpress.com/2009/10/03/static-vs-dynamic-typing-of-programming-languages/)
