# How to hook shared library function calls

* dlsym
* RTLD_NEXT
  > Find the next occurrence of the desired symbol in the search order after the current object. This allows one to provide a wrapper around a function in another shared object, so that, for example, the definition of a function in a preloaded shared object (see LD_PRELOAD in ld.so(8)) can find and invoke the "real" function provided in another shared object (or for that matter, the "next" definition of the function in cases where there are multiple lay‐ ers of preloading). 

* linux equivalent of DllMain
  >  1. No, there is no equivalent to DllMain.
  >  2. For JNI libraries, e.g. on Android, there may be a special entry JNI_OnLoad which is intended to fill JNI function table.
  >  3. GCC defines special attribute constructor to allow some code to run on shared library load.
  >  4. C++ guarantees that the constructors for global and static objects will be performed, no matter if the code that loaded the .so was aware of these classes, or had notion of construction.
  >  5. Same holds for destructors, but there may be unhappy circumstances when at least some destructors have no chance to run - e.g. when there is a sigfault and exceptions are disabled.


TBD

## references
[Function Hooking Part I: Hooking Shared Library Function Calls in Linux](https://blog.netspi.com/function-hooking-part-i-hooking-shared-library-function-calls-in-linux/)
[Let’s Hook a Library Function](https://opensourceforu.com/2011/08/lets-hook-a-library-function/)
[Linux equivalent of DllMain](https://stackoverflow.com/questions/12463718/linux-equivalent-of-dllmain)
