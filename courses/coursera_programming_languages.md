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
### Part A:
* Syntax vs. semantics vs. idioms vs. libraries vs. tools
* ML basics (bindings, conditionals, records, functions)
* Recursive functions and recursive types
* Benefits of no mutation
* Algebraic datatypes, pattern matching
* Tail recursion
* Higher-order functions; closures
* Lexical scope
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
