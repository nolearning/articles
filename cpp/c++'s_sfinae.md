# SFINAE

```c++
// just function
template<typename T> char isClass(int T::*) { return 1; };
template<typename T> int isClass( T ) { return 0; };

// template class
template <typename T>
class IsClassT {
  private:
    typedef char One;
    typedef struct {char a[2];} Two;
    // below not working as T specified, will cause the below two methods to instantiate, it will cause error.
    // static One test(int T::*);
    // static Two test(T);
    template<typename C>
    static One test(int C::*);
    template<typename C>
    static Two test(...);
  public:
    enum { Yes = sizeof(IsClassT<T>::test<T>(0)) == 1 };
    enum { No = !Yes };
};

template <int>
class check {
public:
  static const bool value = 0;
};

template <> 
class check<1> {
  public:
    static const bool value = 1;
};

int main() {
  // 0 in this sentence is nullptr, so int Car::*p = nullprt;
  std::cout << check<sizeof(isClass<Car>(0))>::value << std::endl;
  // 0 in this sentence is 0, so int i = 0;
  std::cout << check<sizeof(isClass<int>(0))>::value << std::endl;
}
```

## References
* [An introduction to C++'s SFINAE concept: compile-time introspection of a class member](http://jguegant.github.io/blogs/tech/sfinae-introduction.html)
* [Notes on C++ SFINAE](https://www.bfilipek.com/2016/02/notes-on-c-sfinae.html)
* [cppreference SFINAE](https://en.cppreference.com/w/cpp/language/sfinae)
* [C++ SFINAE examples?](https://stackoverflow.com/questions/982808/c-sfinae-examples/989518#989518)
* [Implementation Challenge: A count leading zeroes function](https://foonathan.net/blog/2016/02/11/implementation-challenge-2.html)
* [Controlling overload resolution #3: Tag dispatching](https://foonathan.net/blog/2015/11/16/overload-resolution-3.html)
