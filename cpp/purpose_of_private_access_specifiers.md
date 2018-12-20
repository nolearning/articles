# private类访问控制的意义

面向对象编程语言通常具有的三大特性：封装（encapsulation）、继承（inheritance）和多态（polymorphism）

C++ private的类访问控制是封装特性的体现，在编译期限制了成员变量和函数的可见性，益处：
* 对开发人员减少了心智负担
  * 让使用者避免误用，减少对内部实现的关注，只需关注public接口部分
  * 而提供者无需考虑private部分外部如何使用，便于长期维护
* 对编译器来说，是一种零开销的编译时检查工具（静态语言的设计思路，很多时候就是为了让编译器尽可能多的检查出错误）

参考：
* [C++ 类当中为什么要有private？](https://www.zhihu.com/question/21151015)
* [Why make class members private?](https://stackoverflow.com/questions/24661886/why-make-class-members-private)
