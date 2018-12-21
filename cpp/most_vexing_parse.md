# Most vexing parse
The most vexing parse is a specific form of syntactic ambiguity resolution in the C++ programming language. 

## function declaration
```
void f1();
void f2(int val);
void f3(int(val));
void f4(int());
void f5(int);
//void f6((int())) // !error
```

## examples
```
class A {
public:
  A() {}
  A(int a) {}
}
int main {
 A a(); // most vexing parse, to be interpreted as function
 A a(1); // not ambiguous, call class constructor
}

class Timer {
 public:
  Timer();
};

class TimeKeeper {
 public:
  TimeKeeper(const Timer& t);

  int get_time();
};

int main() {
  TimeKeeper time_keeper(Timer()); // most vexing parse
  return time_keeper.get_time();
}

void f(double adouble) {
  int i(int(adouble)); // most vexing parse, same as int i(int adouble)
  int i((int) adouble); // declares a variable called 'i'
  int i(static_cast<int>(adouble)); // same as above
}
```

## references
* [Most vexing parse](https://en.wikipedia.org/wiki/Most_vexing_parse)
* [cppreference Function declaration](https://en.cppreference.com/w/cpp/language/function)
* [C++ thread，构造函数析构函数都没有执行？](https://www.zhihu.com/question/60504977/answer/177197464)
