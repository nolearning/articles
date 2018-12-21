# lvalue and rvalue

## history

## value category

      glvalue        rvalue
   lvalue      xvalue       prvalue

## rvalue reference & lvalue reference


i 表示被识别唯一性（比如地址或者指针，能确定两份拷贝是否同一。）
m 表示可以被移动（允许某种copy在一个临时但是有效的状态.。）
相应的，大写I表示不可以被唯一识别，大写M对应之。

glvalue: 可以被识别唯一性.
rvalue: 允许copy临时有效.
lvalue: 可以被识别唯一性同时他的存在性不是临时的.
xvalue: 可以识别唯一,并且是个临时的状态.
prvalue: 不能被用户识别,但是临时有效.

```c++
Derived(Derived const & rhs) 
  : Base(rhs)
{
  // Derived-specific stuff
}
The version for rvalues has a big fat subtlety. Here's what someone who is not aware of the if-it-has-a-name rule might have done:
Derived(Derived&& rhs) 
  : Base(rhs) // wrong: rhs is an lvalue
{
  // Derived-specific stuff
}
If we were to code it like that, the non-moving version of Base's copy constructor would be called, because rhs, having a name, is an lvalue. What we want to be called is Base's moving copy constructor, and the way to get that is to write
Derived(Derived&& rhs) 
  : Base(std::move(rhs)) // good, calls Base(Base&& rhs)
{
  // Derived-specific stuff
}

// Consider the following function definition:
X foo()
{
  X x;
  // perhaps do something to x
  return x;
}

// Now suppose that as before, X is a class for which we have overloaded the copy constructor and copy assignment operator to implement move semantics. 
// If you take the function definition above at face value, you may be tempted to say, wait a minute, there is a value copy happening here from x to the location of foo's return value. Let me make sure we're using move semantics instead:
X foo()
{
  X x;
  // perhaps do something to x
  return std::move(x); // making it worse!
}
```
## References
* [“New” Value Terminology](http://www.stroustrup.com/terminology.pdf)
* [cppreference value category](https://en.cppreference.com/w/cpp/language/value_category)


左值，右值？
https://zhuanlan.zhihu.com/p/26712193

为什么常量左值引用可以绑定到右值？
https://www.zhihu.com/question/40238995/answer/85890881

C++的右值引用有坑吗？
https://www.zhihu.com/question/56619576

C++ 中的左值、右值、左值引用、右值引用、引用分别是什么，有哪些关系？
https://www.zhihu.com/question/28039779

C++ Rvalue References Explained
http://thbecker.net/articles/rvalue_references/section_01.html

Lvalues, rvalues and references
https://akrzemi1.wordpress.com/2011/11/09/lvalues-rvalues-and-references/

Template parameters and template arguments
https://en.cppreference.com/w/cpp/language/template_parameters


Template argument deduction
https://en.cppreference.com/w/cpp/language/class_template_argument_deduction


https://eli.thegreenplace.net/2011/12/15/understanding-lvalues-and-rvalues-in-c-and-c/

