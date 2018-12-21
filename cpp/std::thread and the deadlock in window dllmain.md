# std::thread and the deadlock in windows dllmain

## std:thread implementation

std::thread has different implementations under windows and linux. 
libstdc++ or libc++ just call pthread, if fail throw exception, but msvc will wait the thread started.

[libc++ (llvm version)](https://github.com/llvm-mirror/libcxx/blob/master/include/thread)
```c++
template <class _Fp, class ..._Args,
          class
         >
thread::thread(_Fp&& __f, _Args&&... __args)
{
    typedef unique_ptr<__thread_struct> _TSPtr;
    _TSPtr __tsp(new __thread_struct);
    typedef tuple<_TSPtr, typename decay<_Fp>::type, typename decay<_Args>::type...> _Gp;
    _VSTD::unique_ptr<_Gp> __p(
            new _Gp(std::move(__tsp),
                    __decay_copy(_VSTD::forward<_Fp>(__f)),
                    __decay_copy(_VSTD::forward<_Args>(__args))...));
    int __ec = __libcpp_thread_create(&__t_, &__thread_proxy<_Gp>, __p.get());
    if (__ec == 0)
        __p.release();
    else
        __throw_system_error(__ec, "thread constructor failed");
}
```

[libstdc++ (gnu 8.2.0)](https://gcc.gnu.org/onlinedocs/gcc-8.2.0/libstdc++/api/a00158_source.html).  
[libstdc++-v3 thread.cc](https://code.woboq.org/gcc/libstdc++-v3/src/c++11/thread.cc.html)
```c++
 template<typename _Callable, typename... _Args>
       explicit
       thread(_Callable&& __f, _Args&&... __args)
       {
 #ifdef GTHR_ACTIVE_PROXY
    // Create a reference to pthread_create, not just the gthr weak symbol.
    auto __depend = reinterpret_cast<void(*)()>(&pthread_create);
 #else
    auto __depend = nullptr;
 #endif
         _M_start_thread(_S_make_state(
          __make_invoker(std::forward<_Callable>(__f),
                         std::forward<_Args>(__args)...)),
        __depend);
  }
  // thread.cc
  extern "C"
  {
    static void*
    execute_native_thread_routine(void* __p)
    {
      thread::_State_ptr __t{ static_cast<thread::_State*>(__p) };
      __t->_M_run();
      return nullptr;
     }
  }
  thread::_M_start_thread(_State_ptr state, void (*)())
  {
    const int err = __gthread_create(&_M_id._M_thread,
                                     &execute_native_thread_routine,
                                     state.get());
    if (err)
      __throw_system_error(err);
    state.release();
  }
```

[ms vc19](https://github.com/icestudent/vc-19-changes/)
```c++
// thread
template<class _Fn,
		class... _Args,
		class = typename enable_if<
			!is_same<typename decay<_Fn>::type, thread>::value>::type>
		explicit thread(_Fn&& _Fx, _Args&&... _Ax)
		{	// construct with _Fx(_Ax...)
		_Launch(&_Thr,
			_STD make_unique<tuple<decay_t<_Fn>, decay_t<_Args>...> >(
				_STD forward<_Fn>(_Fx), _STD forward<_Args>(_Ax)...));
}

// thr/xthread
template<class _Target> inline
	void _Launch(_Thrd_t *_Thr, _Target&& _Tg)
	{	// launch a new thread
	_LaunchPad<_Target> _Launcher(_STD forward<_Target>(_Tg));
	_Launcher._Launch(_Thr);
}

...
void _Launch(_Thrd_t *_Thr)
		{	// launch a thread
		_Thrd_startX(_Thr, _Call_func, this);
		while (!_Started)
			_Cnd_waitX(_Cond, _Mtx);
}
...
static _Call_func_ret _STDCALL _Call_func(void *_Data)
		{	// entry point for new thread
		static_cast<_Pad *>(_Data)->_Go();
		_Cnd_do_broadcast_at_thread_exit();
		return (0);
		}

	_Cnd_t _Cond;
	_Mtx_t _Mtx;
	bool _Started;
};

// _LaunchPad::_Go 
virtual void _Go()
		{	// run the thread function object
		_Run(this);
}

static void _Run(_LaunchPad *_Ln) _NOEXCEPT	// enforces termination
		{	// construct local unique_ptr and call function object within
		_Target _Local(_STD forward<_Target>(_Ln->_MyTarget));
		_Ln->_Release();
		_Execute(*_Local,
			make_integer_sequence<size_t,
				tuple_size<typename _Target::element_type>::value>());
}
```

## why std:thread call in dllmain will cause deadlock

### loader lock.
loader lock is a process-wide lock used by the system to synchronize access to loading DLL's into a process address space.

dllmain function runs inside the loader lock, one of the few times the OS lets you run code while one of its internal locks is held. 

As the code in previous section, msvc std::thread will wait the created thread enter the thread's routine.
So the thread A call std::thread in dllmain held the loader lock and wait thread B thread B created by std::thread to enter the routine passed by thread A.  
While thread B will call dllmain again with DLL_THREAD_ATTACH event as the windows system api description, so thread B will wait the loader lock.
At the end, the deadlock happended.

| thread A | thread B |
| -------- | -------- |
| load dll |          |
| held the loader lock |         |
| l call std::thread to create thread B in the dllmain |          |
|          |  started, os api will call dllmain with DLL_THREAD_ATTACH |
|  |   blocked, try to hold the loader lock      |
| blocked, waiting for thread B call the routine |          |

## Things should not do in DllMain

according the microsoft document
> dllmain function runs inside the loader lock, one of the few times the OS lets you run code while one of its internal locks is held. 
> 
> At system loading library(via LoadLibrary or implicitly loading), apis (GetModuleFileName, 
>
> You should never perform the following tasks from within DllMain:  
> * Call LoadLibrary or LoadLibraryEx (either directly or indirectly). This can cause a deadlock or a crash.
> * Call GetStringTypeA, GetStringTypeEx, or GetStringTypeW (either directly or indirectly). This can cause a deadlock or a crash.
> * Synchronize with other threads. This can cause a deadlock.
> * Acquire a synchronization object that is owned by code that is waiting to acquire the loader lock. This can cause a deadlock.
> * Initialize COM threads by using CoInitializeEx. Under certain conditions, this function can call LoadLibraryEx.
> * Call the registry functions. These functions are implemented in Advapi32.dll. If Advapi32.dll is not initialized before your DLL, the DLL can access uninitialized memory and cause the process to crash.
> * Call CreateProcess. Creating a process can load another DLL.
> * Call ExitThread. Exiting a thread during DLL detach can cause the loader lock to be acquired again, causing a deadlock or a crash.
> * Call CreateThread. Creating a thread can work if you do not synchronize with other threads, but it is risky.
> * Create a named pipe or other named object (Windows 2000 only). In Windows 2000, named objects are provided by the Terminal Services DLL. If this DLL is not initialized, calls to the DLL can cause the process to crash.
> * Use the memory management function from the dynamic C Run-Time (CRT). If the CRT DLL is not initialized, calls to these functions can cause the process to crash.
> * Call functions in User32.dll or Gdi32.dll. Some functions load another DLL, which may not be initialized.
    Use managed code.

### why
* may make deadlock, such as the before 
* may make the object associate with thread wrong

## references
* [standard --  Thread support library](http://eel.is/c++draft/thread.thread.constr)
* [man page -- pthread_create](http://man7.org/linux/man-pages/man3/pthread_create.3.html)
* [Windows DllMain entry point](https://docs.microsoft.com/en-us/windows/desktop/dlls/dllmain)
* [windows CreateThread function](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createthread)

* [Why does std::thread wait in its constructor?](https://stackoverflow.com/questions/42579625/why-does-stdthread-wait-in-its-constructor)
* [std::thread cause deadlock in DLLMain](https://stackoverflow.com/questions/32252143/stdthread-cause-deadlock-in-dllmain)
* [Loader lock error](https://stackoverflow.com/questions/56642/loader-lock-error)
* [Why am I getting the “LoaderLock was detected” warning when debugging?](https://stackoverflow.com/questions/889736/why-am-i-getting-the-loaderlock-was-detected-warning-when-debugging)

* [Dynamic-Link Library Best Practices](https://docs.microsoft.com/en-us/windows/desktop/dlls/dynamic-link-library-best-practices)
* [Some reasons not to do anything scary in your DllMain](https://blogs.msdn.microsoft.com/oldnewthing/20040127-00/?p=40873)
* [Another reason not to do anything scary in your DllMain: Inadvertent deadlock](https://blogs.msdn.microsoft.com/oldnewthing/20040128-00/?p=40853)
* [Does creating a thread from DllMain deadlock or doesn’t it?](https://blogs.msdn.microsoft.com/oldnewthing/20070904-00/?p=25283/)
* [How you might be loading a DLL during DLL_PROCESS_DETACH without even realizing it](https://blogs.msdn.microsoft.com/oldnewthing/20100115-00/?p=15253/)



