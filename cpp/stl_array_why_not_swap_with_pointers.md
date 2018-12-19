# 为什么std:array交换时不能用指针——std:array的实现简析
std::array本身是值类型，在标准库的实现中为struct结构体类型，包含特定类型的数组成员。
例如std::array<int, 4>模板实化例后，简化结构如下：
```c++
struct array_int_four {
  int elements[4];
};
```
 std::array的swap函数实质就是对两个数组实体进行交换，由于c++数组的内存地址确定后不能再改变，那么只能交换内容。
为什么内部数组成员不用指针方式实现，这可能是从语义、性能上的考虑。参考
[std::array - cppreference.com](https://en.cppreference.com/w/cpp/container/array)
>std::array is a container that encapsulates fixed size arrays.   
>**This container is an aggregate type with the same semantics as a struct holding a C-style array T[N] as its only non-static data member.** Unlike a C-style array, it doesn't decay to T* automatically. As an aggregate type, it can be initialized with aggregate-initialization given at most N initializers that are convertible to `T: std::array<int, 3> a = {1,2,3};`.   
>**The struct combines the performance and accessibility of a  C-style array with the benefits of a standard container,** such as knowing  its own size, supporting assignment, random access iterators, etc. 

附录std::array源码

[libstdc++下array源码（gcc 8.2.0）](https://gcc.gnu.org/onlinedocs/gcc-8.2.0/libstdc++/api/a00041_source.html)   
摘要如下：
```c++
template<typename _Tp, std::size_t _Nm>
  struct __array_traits
{
  typedef _Tp _Type[_Nm];   //自定义长度为_Nm的数组类型
  ....
};
template<typename _Tp, std::size_t _Nm>
  struct array
{
  ...
  typedef _GLIBCXX_STD_C::__array_traits<_Tp, _Nm> _AT_Type;
  typename _AT_Type::_Type _M_elems;       //声明自定义数组类型成员
  ...
};
```
[libc++下array源码（llvm）](https://github.com/llvm-mirror/libcxx/blob/master/include/array)  
摘要如下：
```c++
template <class _Tp, size_t _Size>
struct _LIBCPP_TEMPLATE_VIS array
{
...
  _Tp __elems_[_Size]; //声明类型为_Tp长度为_Size的数组
...
};
```
