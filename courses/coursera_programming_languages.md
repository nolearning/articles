# [Programing Languages by University of Washington](https://www.coursera.org/learn/programming-languages/)

## Introduction
> This course is an introduction to the basic concepts of programming languages, with a strong emphasis on functional programming. 
> 
> The course will give you a framework for understanding how to use language constructs effectively and how to design correct and elegant programs. By using different languages, you will learn to think more deeply than in terms of the particular syntax of one language. The emphasis on functional programming is essential for learning how to write robust, reusable, composable, and elegant programs. Indeed, many of the most important ideas in modern languages have their roots in functional programming. 
### Tools
* Emacs
  * brew cask install emacs
  * brew install emacs --HEAD --with-cocoa --with-gnutls --with-librsvg --with-imagemagick@6 --with-mailutils 
  * [How can I install emacs correctly on OS X?](https://stackoverflow.com/a/47155790)
  * [Emacs For Mac OS](https://www.emacswiki.org/emacs/EmacsForMacOS)
  * [A Guided Tour of Emacs](http://www.gnu.org/software/emacs/tour/)
  * [The Emacs Editor](http://www.gnu.org/software/emacs/manual/html_node/emacs/index.html)
  * [Emacs Wiki](https://www.emacswiki.org/)
  * [MELPA repository](https://github.com/melpa/melpa)
  * Install `exec-path-from-shell` package
    * [exec-path-from-shell](https://github.com/purcell/exec-path-from-shell)
    ```lisp
    (when (memq window-system '(mac ns x))
    (exec-path-from-shell-initialize))
    ```
    * [Why Emacs overrides my PATH when it runs bash?](https://emacs.stackexchange.com/questions/14159/why-emacs-overrides-my-path-when-it-runs-bash)
* SML/NJ
  * brew install smlnj
  * [Painless installation of SML on OS X](http://islovely.co/posts/painless-installation-of-sml-on-os-x/)
  * [Standard ML of New Jersey](http://www.smlnj.org/)
  * SML Mode For Emacs
    * Run `M-x list-packages` -> Find `sml-mode` -> click to select then click install button
    * Install manually
      1. download sml-mode extension from [GNU ELPA - sml-mode](http://elpa.gnu.org/packages/sml-mode.html)
      2. In Emacs using ` M-x package-install-file ENTER`
      3. select the download package
      4. restart emacs

## Course Topics
### Part A (SML):
#### Syntax & sematics & environment
* static environment & dynamic environment  
  static environment are build up by elaboration and dynamic environment by evaluation of declarations.  
  What type (if any) a binding has depends on a static  environment, which is roughly the types of the preceding bindings in the file.  How a binding is evaluated depends on a dynamic environment, which is roughly the values of the preceding bindings in the file.  
```sml
val x = 34;
(* static environment: x : int *)
(* dynamic environment: x --> 34 *)
val y = 17;
(* static environment: x : int, y : int *)
(* dynamic environment: x --> 34, y --> 17 *)
```
1. syntax -- how to write something / How do you write language constructs?
2. semantics -- what that something means / What do programs mean? (Evaluation rules)
   - Type-checking(before program runs)
     - Valid Types: what types are allowed for subexpressions, and what type this expression returns
   - Evaluation (as program runs), how we run this when it's part of a program
3. Idioms -- What are typical patterns for using language features to express your computation?
4. Libraries -- What facilities does the language (or a well-known project) provide “standard”? (E.g., file access, data structures)
5. Tools: What do language implementations provide to make your job easier? (E.g., REPL, debugger, code formatter, ...)
> focus on semantics and idioms
> * Syntax is usually uninteresting
>   - A fact to learn, like “The American Civil War ended in 1865”
>   - People obsess over subjective preferences
> * Libraries and tools crucial, but often learn new ones “on the job”
>   - We are learning semantics and how to use that knowledge to understand all software and employ appropriate idioms
>   - By avoiding most libraries/tools, our languages may look “silly” but so would anylanguage used this way

* Early Binding vs. Late Binding 
  - Early binding
    - type is known before the variable is exercised during run-time, usually through a static, declarative means
  - Late binding
    - Late binding, dynamic binding[1], or dynamic linkage[2] is a computer programming mechanism in which the method being called upon an object or the function being called with arguments is looked up by name at runtime.   
      Type is unknown until the variable is exercised during run-time; usually through assignment but there are other means to coerce a type; dynamically typed languages call this an underlying feature, but many statically typed languages have some method of achieving late binding.  
      Implemented often using [special] dynamic types, introspection/reflection, flags and compiler options, or through virtual methods by borrowing and extending dynamic dispatch 
  - [Late binding](https://en.wikipedia.org/wiki/Late_binding)
  - [What is early and late binding?](https://softwareengineering.stackexchange.com/questions/200115/what-is-early-and-late-binding)
  
* Static/Dynamic and Strong/Weak Type
  - Static Type vs. Dynamic Type
    - Static/Dynamic Typing is about when type information is acquired (Either at compile time or at runtime)
    - Static type checking is the process of verifying the type safety of a program based on analysis of a program's text (source code). If a program passes a static type checker, then the program is guaranteed to satisfy some set of type safety properties for all possible inputs.
    - Dynamic type checking is the process of verifying the type safety of a program at runtime.
    - In a statically typed language, every variable name is bound both
      - to a type (at compile time, by means of a data declaration)
      - to an object.
      - The binding to an object is optional — if a name is not bound to an object, the name is said to be null.
    - In a dynamically typed language, every variable name is (unless it is null) bound only to an object.
      - Names are bound to objects at execution time by means of assignment statements, and it is possible to bind a name to objects of different types during the execution of the program.
      - Once a variable name has been bound to a type (that is, declared) it can be bound (via an assignment statement) only to objects of that type; it cannot ever be bound to an object of a different type. An attempt to bind the name to an object of the wrong type will raise a type exception. 
  - Strong Type vs. Weak Type
    - Strong/Weak Typing is about how strictly types are distinguished (e.g. whether the language tries to do an implicit conversion from strings to numbers).
    - "Strong typing" generally refers to use of programming language types in order to both capture invariants of the code, and ensure its correctness, and definitely exclude certain classes of programming errors. Thus there are many "strong typing" disciplines used to achieve these goals.
    - Some programming languages make it easy to use a value of one type as if it were a value of another type. This is sometimes described as "weak typing".
    - In a weakly typed language, variables can be implicitly coerced to unrelated types, whereas in a strongly typed language they cannot, and an explicit conversion is required.
   - [Type system](https://en.wikipedia.org/wiki/Type_system)
   - [Strong and weak typing](https://en.wikipedia.org/wiki/Strong_and_weak_typing)
   - [Static/Dynamic vs Strong/Weak](https://stackoverflow.com/questions/2351190/static-dynamic-vs-strong-weak)
   - [Static vs. dynamic typing of programming languages](https://pythonconquerstheuniverse.wordpress.com/2009/10/03/static-vs-dynamic-typing-of-programming-languages/)

* Expressions
  - Many kinds of expressions: `34, true, e1 + e2, if e1 then e2 else e3`
  - Can get arbitarily large since any subexpression can contain subsubexpressions, etc.
  - Every kind of expression has
    1. Syntax
    2. Type-checking rules
       * Produces a type or fails (with a bad error message)
       * E.g. `int bool unit`
    3. Evaluation rules (used only on things that type-check)
       * Produces a value (or exception or infinite-loop)
* Variables
  - Syntax:  
    sequence of letters, digits, _, not starting with digit
  - Type-checking:   
    Look up type in current static environment
    If not there, fail
  - Evaluation:   
    Look up value in current dynamic environment
  - For variable bindings
    - Type-check expression and extend *static environment*
    - Evaluate expression and extend *dynamic environment*
* Addition
  - Syntax: *e1 + e2* where *e1* and *e2* are expressions
  - Type-checking:  
    if *e1* and *e2* have type int, then *e1 + e2* has type int.
  - Evaluation  
    - if *e1* evaluates to *v1* and *e2* evaluates to *v2*, then *e1 + e2* evaluates to sum of *v1* and *v2*
* Values
  - All values are expressions, not all expressions are values
  - Every value "evaluates to itself"
  - E.g. `34, 17` have type int, `true, false` have type bool, `()` has type unit
    
* REPL(Read-Eval-Print-Loop)
  - `use "some.sml"` enters bindings from the file `some.sml`
* Errors
  - Syntax: What you wrote means nothing or not the construct you intended
  - Type-checking: What you wrote does not type-check
  - Evaluation: It runs but produces wrong answer, or an exception, or an infinite loop
* Shadowing
  - Expressions in variable bindings are evaluated “eagerly”
    - Before the variable binding “finishes”
    - Afterwards, the expression producing the value is irrelevant
  - There is no way to “assign to” a variable in ML
    - Can only shadow it in a later environment
* Function

* Syntax vs. semantics vs. idioms vs. libraries vs. tools
* ML basics (bindings, conditionals, records, functions)
* Recursive functions and recursive types
* Benefits of no mutation
* Algebraic datatypes, pattern matching
* Tail recursion
* Higher-order functions; closures
* Lexical scope  
  Bindings in ML live in environments. Conceptually, each environment (except the top-level environment) consists of
  - a list of name-value pairs (the bindings in this environment); and
  - a pointer to the "parent" environment. This pointer refers to the textually enclosing environment at the place where the environment is first defined. (The top-level environment differs only in that it does not have a parent pointer.)
  - Lexical vs. dynamic scoping
    The scoping rule used in ML is called lexical scoping, because names refer to their nearest preceding lexical (textual) definition.  
    The opposite scheme is called dynamic scoping --- under dynamic scoping, names from outer scopes are re-evaluated using the most recently executed definition, not the one present when the code was originally written.
* [More on scoping of names](https://courses.cs.washington.edu/courses/cse341/04wi/lectures/05-ml-scoping.html)
* Currying
* Syntactic sugar
* Equivalence and effects
* Parametric polymorphism and container types
* Type inference
* Abstract types and modules

### Part B:
* Racket basics
* Dynamic vs. static typing
* Laziness, streams, and memoization
* Implementing languages, especially higher-order functions
* Macros
* Eval

### Part C:
* Ruby basics
* Object-oriented programming is dynamic dispatch
* Pure object-orientation
* Implementing dynamic dispatch
* Multiple inheritance, interfaces, and mixins
* OOP vs. functional decomposition and extensibility
* Subtyping for records, functions, and objects
* Class-based subtyping
* Subtyping
* Subtyping vs. parametric polymorphism; bounded polymorphism
