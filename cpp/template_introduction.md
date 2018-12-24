# Template Introduction
## What's Template

> A template is a C++ entity that defines one of the following:
>  * a family of classes (class template), which may be nested classes
>  * a family of functions (function template), which may be member functions
>  * an alias to a family of types (alias template) (since C++11)
>  * a family of variables (variable template) (since C++14)
>  * a concept (constraints and concepts) (since C++20) 
* [cppreference Templates](https://en.cppreference.com/w/cpp/language/templates)

## Templates Partial Specialization


### Function template partial specialization
NO. Why? most can write overloaded one. Should not mix specialization and overloading
```c++
template <typename T> void f(T ){std::cout << "#1" << std::endl;} // #1
template <typename T> void f(T* ){std::cout << "#2" << std::endl;} // #2
template <> void f<>(int* ){std::cout << "#3" << std::endl;}
f((int*)0); //==> #3

//declare in other order
template <typename T> void f(T ){std::cout << "#1" << std::endl;} // #1
template <> void f<>(int* ){std::cout << "#3" << std::endl;}
template <typename T> void f(T* ){std::cout << "#2" << std::endl;} // #2
f((int*)0); //==> #2
```
* [Function Templates Partial Specialization in C++](https://www.fluentcpp.com/2017/08/15/function-templates-partial-specialization-cpp/)

## Variadic templates
* [Variadic templates in C++](https://eli.thegreenplace.net/2014/variadic-templates-in-c/)

## References
* [What's Wrong with C++ Templates?](http://people.cs.uchicago.edu/~jacobm/pubs/templates.html)
* [How I Learned to Stop Worrying and Love the Type System](http://reasonablypolymorphic.com/blog/love-types/)
