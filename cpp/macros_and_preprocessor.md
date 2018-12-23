# Macros And Preprocessor

## Macros
* Object-like Macros
  * `#define NUM 1`
* Function-like Macros
  * `#define mfunc(a, b) (a) + (b)`
  * A function-like macro is only expanded if its name appears with a pair of parentheses after it.
    ```c
    #define foo() xxxx
    foo(); -> xxxx
    x = foo; //just let it alone
    ```
  * Is there are spaces between the macro name and the parentheses, it defines an object-like macro whose expansion happens to begin with a pair of parentheses
    ```c
    #define lang_init ()    c_init()
    lang_init()
      → () c_init()()
    ```
* onto multi lines
  ```c
  #define NUMBERS 1, \
                2, \
                3
  int x[] = { NUMBERS };
     → int x[] = { 1, 2, 3 };
  ```
* When the preprocessor expands a macro name, the macro’s expansion replaces the macro invocation, then the expansion is examined for more macros to expand
  ```c
  #define VERSION 1
  #define OS_VERSION VERSION
  // OS_VERSION -> VERSION -> 1, 
  ```
 * Self-Referential Macros
 
   A self-referential macro is one whose name appears in its definition. The self-reference is not considered a macro call. 
   ```c
   #define foo (4 + foo)
   foo
      → 4 + foo //not infinitely large expansion
 
    #define x (4 + y)
    #define y (2 * x) 
    x    → (4 + y)
       → (4 + (2 * x))
    y    → (2 * x)
         → (2 * (4 + y))
    ```
  * stringizing(#) and concatenation(##)
    ```c
    //se two levels of macros to stringize the result of expansion of a macro argument,
    #define xstr(s) str(s)
    #define str(s) #s
    #define foo 4
    str (foo)
         → "foo"
    xstr (foo)
         → xstr (4)
         → str (4)
         → "4"
    #define COMMAND(NAME)  { #NAME, NAME ## _command }

      struct command commands[] =
      {
        COMMAND (quit),
        COMMAND (help),
        …
      };
      ->
      struct command commands[] =
      {
        { "quit", quit_command },
        { "help", help_command },
        …
      };
     ```
  * Vardic Macros
    ```c
    #define eprintf(…) fprintf (stderr, __VA_ARGS__)
    eprintf ("%s:%d: ", input_file, lineno)
     →  fprintf (stderr, "%s:%d: ", input_file, lineno)
    ```
    * name for the variable argument -- `#define eprintf(args…) fprintf (stderr, args)`
  * Predefined Macros
    * Standard Predefined Macros, Common Predefined Macros, System-specific Predefined Macros, C++ Named Operators
    ```
    clang/gcc -dM -E -x c /dev/null
    clang++/g++ -dM -E -x c++ /dev/null
    # "-dM" dumps a list of macros.
    # "-E" prints results to stdout instead of a file.
    # "-x c" and "-x c++" select the programming language when using a file without a filename extension, such as /dev/null. If you instead compile "dummy.c" and "dummy.cpp" files, these options aren't needed.
    ```
  * output preprocessed source code
  ```
  gcc/clang/g++/clang++ -E some.c
  cpp some.c //cpp under clang not right
  ```
  * macro argument
    * don't have balanced parentheses
      ```c
      #define strange(file) fprintf (file, "%s %d",
      …
      strange(stderr) p, 35)
           → fprintf (stderr, "%s %d", p, 35)
      ```
   * macro not guarentee the precedence, so each occurrence of a macro argument name should have parentheses around it
     ```c
     #define ceil_div(x, y) (x + y - 1) / y
     a = ceil_div (b & c, sizeof (int));
       → a = (b & c + sizeof (int) - 1) / sizeof (int);
     // not below
     a = ((b & c) + sizeof (int) - 1)) / sizeof (int);
     //should
     #define ceil_div(x, y) ((x) + (y) - 1) / (y)
     ```
   * the semicolon problem
     use a macro like a function call, with or without semicolon maybe a problem
     ```c
     #define FUNC(p) { do_something }
     if (1) 
      FUNC(p); //wrong semicolon 
     else...
     ==> if (1) { do_something }; else... //wrong
     //should
     #define FUNC(p) do {do_something} while(0)
     ```
   * Duplication of Side Effects
     ```c
     #define min(X, Y)  ((X) < (Y) ? (X) : (Y))
     next = min (x + y, foo (z));
      ==> next = ((x + y) < (foo (z)) ? (x + y) : (foo (z))); //x + y duplicated
     ```
   * newlines in arguments
     The invocation of a function-like macro can extend over many logical lines. However, in the present implementation, the entire expansion comes out on one line. Thus line numbers emitted by the compiler or debugger refer to the line the invocation started on, which might be different to the line containing the argument causing the problem.
    * Argument prescan
      Macro arguments are completely macro-expanded before they are substituted into a macro body, unless they are stringized or pasted with other tokens.After substitution, the entire macro body, including the substituted arguments, is scanned again for macros to be expanded. The result is that the arguments are scanned twice to expand macro calls in them.
      * For nested calls to a macro
        Nested calls to a macro.
We say that nested calls to a macro occur when a macro’s argument contains a call to that very macro. For example, if f is a macro that expects one argument, f (f (1)) is a nested pair of calls to f. The desired expansion is made by expanding f (1) and substituting that into the definition of f. The prescan causes the expected result to happen. Without the prescan, f (1) itself would be substituted as an argument, and the inner use of f would appear during the main scan as an indirect self-reference and would not be expanded.
      * Macros used in arguments, whose expansions contain unshielded commas.
        ```c
        #define foo  a,b
        #define bar(x) lose(x)
        #define lose(x) (1 + (x))
        //expected bar(foo) => (1 + (a,b))
        ==> lose(a, b) //cause error
        //should
        #define foo (a,b) or  #define bar(x) lose((x))

        ```
## References
* [Macros](https://gcc.gnu.org/onlinedocs/cpp/Macros.html)
* [Can gcc output C code after preprocessing?](https://stackoverflow.com/questions/4900870/can-gcc-output-c-code-after-preprocessing)
