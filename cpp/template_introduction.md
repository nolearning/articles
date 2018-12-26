# Template Introduction
## What's Template

> A template is a C++ entity that defines one of the following:
>  * a family of classes (class template), which may be nested classes
>  * a family of functions (function template), which may be member functions
>  * an alias to a family of types (alias template) (since C++11)
>  * a family of variables (variable template) (since C++14)
>  * a concept (constraints and concepts) (since C++20) 
* [cppreference Templates](https://en.cppreference.com/w/cpp/language/templates)

## typename

### Nested templates with dependent scope
dependent scope as the type is in a scope that depends on the instantiation of a template.
The typename disambiguator for dependent names, whether describe a member variable or a nested type.
```c++
struct A { // A has a nested variable X and a nested type struct X
   struct X {};
   int X;
};
struct B {
    struct X { }; // B has a nested type struct X
};
template<class T> void f(T t) {
    typename T::X x;
}
void foo() {
    A a;
    B b;
    f(b); // OK: instantiates f<B>, T::X refers to B::X
    f(a); // error: cannot instantiate f<A>:
          // because qualified name lookup for A::X finds the data member
}
```

## static member in class template
From N3290, 14.6:
> A [...] static data member of a class template shall be defined in every translation unit in which it is implicitly instantiated [...], unless the corresponding specialization is explicitly instantiated [...] .

```c++
template <typename T>
struct A {
  static int num;
};
int main () {
   /* under gcc get a link error, undefined reference to `A<char>::num'
    * under clang will get warning at compiling and error when link
   // warning: instantiation of variable 'A<char>::num' required here, but no definition is available
   // Undefined symbols for architecture x86_64, "A<char>::num", referenced from: main
   return A<char>::num; 
}

// the right way
template <typename T>
struct A {
  static int num; //declaration
};

template <typename T>
int A<T>::num; // definition

// since c++17
template <typename T>
struct A {
   static inline int num;
};
```
why? First, look at static member in class, before c++17, as declare a struct/class with static members, these static members should define in source file, as the c++ **one definition rule**.
```c++
// some header file
struct SA {
  static int num;
};

// some source file
int SA::num;

// since c++17
struct SA {
  inline static int num;
};
```
So, the template class will define the static members outside the template class's declaration, to keep same behaviour when template class initilazation. But, the template static member can define in header while the specilization one only define in source file.
```c++
// in header file
template <typename T>
struct A {
  static int num; //declaration
};

template <typename T>
int A<T>::num; // definition

// below line will get error at link time as redefined in multiple source
template<>
int A<char>::num;
// 
```

* [What should happen to template class static member variables with definition in the .h file](https://stackoverflow.com/questions/7108914/what-should-happen-to-template-class-static-member-variables-with-definition-in)
* [Static member initialization in a class template](https://stackoverflow.com/questions/3229883/static-member-initialization-in-a-class-template)
* [Why does a static data member need to be defined outside of the class?](https://stackoverflow.com/questions/18749071/why-does-a-static-data-member-need-to-be-defined-outside-of-the-class)

## Templates Partial Specialization
* class partial template
```c++
template<typename T>
struct is_pointer : std::false_type{};
template<typenaem T>
struct is_pointer<T*> : std::true_type{};
```
* variable partial template
```
template<typename T>
bool is_pointer_t = false;
template<typename T>
bool is_pointer_t<T*> = true;
```
* standard forbids partial specialization on anything else than classes(or structs) and variables.
```c++
// alias template
template<typename T>
using type = int;
template<typename T>
using type<T*> = int*;

// function template
constexpr bool is_pointer(T const&) {
  return false;
}
constexpr bool is_pointer<T*>(T const&) {
  return true;
}
```
as proposal said
> 2.2 The Main Choice: Specialization vs. Everything Else
> 
> After discussion on the reflectors and in the Evolution WG, it turns out that we have to choose between two mutually exclusive models:
>
> A typedef template is not itself an alias; only the (possibly-specialized) instantiations of the typedef template are aliases. This choice allows us to have specialization of typedef templates.
> 
> A typedef template is itself an alias; it cannot be specialized. This choice would allow:
>
> * deduction on typedef template function parameters (see 2.4)
> * a declaration expressed using typedef templates be the same as the declaration without typedef templates (see 2.5)
> * typedef templates to match template template parameters (see 2.6)

to emulate partial template specialization
```c++
// alias template
template<typename T>
struct typeImpl { using type = int;};
template<typename T>
struct typeImpl<T*> { using type = int*; };
template<typename T>
alias aType = typename typeImpl<T>::type;
// function template
template<typename T>
struct is_pointer_impl { static constexpr bool _() { return false; } };
template<typename T>
struct is_pointer_impl<T*> { static constexpr bool _() { return true; } };
template<typename T>
constexpr bool is_pointer(T const&) {
  return is_pointer_impl<T>::_();
}
```

* [cppreference Dependent names](https://en.cppreference.com/w/cpp/language/dependent_name)
* [Nested templates with dependent scope](https://stackoverflow.com/questions/3311633/nested-templates-with-dependent-scope)
* [cppreference partial template specialization](https://en.cppreference.com/w/cpp/language/partial_specialization)
* [Partial Specialization of Alias Templates](https://stackoverflow.com/questions/21774471/partial-specialization-of-alias-templates/21774990#21774990)
* [](https://www.fluentcpp.com/2017/08/11/how-to-do-partial-template-specialization-in-c/)

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

## boost function&bind

## boost any
[How the Boost Bind Library Can Improve Your C++ Programs(http://www.informit.com/articles/article.aspx?p=412354)
[详细解析boost中bind的实现](https://blog.csdn.net/hengyunabc/article/details/7773250)
[Boost源码剖析](https://blog.csdn.net/pongba/article/category/37521)
[std::bind 的实现原理](http://blog.guorongfei.com/2017/01/27/bind-implementation/)
[How Do Those Funky Placeholders Work?](https://accu.org/index.php/journals/1397)
[std::placeholders::_1, std::placeholders::_2, ..., std::placeholders::_N](https://en.cppreference.com/w/cpp/utility/functional/placeholders)


## Variadic templates
* [Variadic templates in C++](https://eli.thegreenplace.net/2014/variadic-templates-in-c/)

## Expression templates
[Expression templates](https://en.wikipedia.org/wiki/Expression_templates)

## References
* [What's Wrong with C++ Templates?](http://people.cs.uchicago.edu/~jacobm/pubs/templates.html)
* [How I Learned to Stop Worrying and Love the Type System](http://reasonablypolymorphic.com/blog/love-types/)
