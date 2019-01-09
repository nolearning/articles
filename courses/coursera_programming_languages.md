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
* syntax -- how to write something
* semantics -- what that something means
  - Type-checking(before program runs)
  - Evaluation (as program runs)
* For variable bindings
  - Type-check expression and extend *static environment*
  - Evaluate expression and extend *dynamic environment*
* Variables
  - Syntax:  
    sequence of letters, digits, _, not starting with digit
  - Type-checking:   
    Look up type in current static environment
    If not there, fail
  - Evaluation:   
    Look up value in current dynamic environment
* Addition
  - Syntax: *e1 + e2* where *e1* and *e2* are expressions
  - Type-checking:  
    if *e1* and *e2* have type int, then *e1 + e2* has type int.
* Evaluation  
  - if *e1* evaluates to *v1* and *e2* evaluates to *v2*, then *e1 + e2* evaluates to sum of *v1* and *v2*

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
