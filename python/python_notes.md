# Python Notes
## Install
* Mac OSX
  - `brew install python` will install python3
  - `brew link/unlink python` 
  - `brew info python`
  - python3 will be `/usr/local/bin/python3`, python will be python2, because some application in OSX may depend on python2
  - [How to set Python's default version to 3.x on OS X?](https://stackoverflow.com/questions/18425379/how-to-set-pythons-default-version-to-3-x-on-os-x/18425592)
* virtualenv
  1. setup virtualenv and init a proejct (and virtualenvwrapper)
  ```shell
  pip install virtualenv
  cd project_dir
  virtualenv venv
  source venv/bin/activate
  pip install <packages>
  pip freeze > projectDir/requirements.txt # the txt file hold the necessary dependencies for your project
  deactive
  ```
  2. clone project in another machine under virutalenv
  ``` 
  pip install virtualenv # if not installed
  virtualenv project_dir && source project_dir/venv/bin/activate
  git clone
  cd project_dir
  pip -r reuiqrements.txt
  ```
  
## Inheritance

### super
`super()` lets you avoid referring to the base class explicitly. In python 3, just say `super().__init__()` instead of `super(Child, self).__init__()`.

```python
class MyParentClass(object):
  def __init__(self):
    pass

# python 2
class SubClass(MyParentClass):
  def __init__(self):
    super(SubClass, self).__init__()
    
# python 3
class SubClass(MyParentClass):
  def __init__(self):
    super()
```

* [Working with the Python Super Function](https://www.pythonforbeginners.com/super/working-python-super-function)

## Formatting
* % - old style
* '{}'.format - new style

* [Using % and .format() for great good!](https://pyformat.info/)

## Decorators
```python
def log(func):
  def decorator(*args, **kwargs):
    print('Log: function {} is calling'.format(func.__name__))
    return func(*args, **kwargs)
  return decorator

def tag(tagName):
  def tag_decorator(func):
    def decorator(*args, **kwargs):
      return '<{0}>{1}</{0}>'.format(tagName, func(*args, **kwargs))
    return decorator
  return tag_decorator

@tag('p')
@log
def greet(name):
  return 'Hello {}'.format(name)
```

* [Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)
* [A guide to Python's function decorators](https://www.thecodeship.com/patterns/guide-to-python-function-decorators/)

## Modules
* A module is a file containing Python definitions and statements.
* `import module as alias_module`
* `from package import module` or `from module import objects`
* \_\_name\_\_ is the module’s name
* module search path
  1. searches for a built-in module with that name
  2. a list of directories given by the variable sys.path. sys.path
    - the directory containing the input script (or the current directory).
    - PYTHONPATH (a list of directory names, with the same syntax as the shell variable PATH).
    - the installation-dependent default.
    - user customized path
* byte-compiled python files
  - if `module.pyc` file exists in the directory where `module.py` exists, and the modification time of `module.py` matched with `module.pyc`, then the pyc file will load
  - When the Python interpreter is invoked with the -O flag, optimized code is generated and stored in .pyo files. The optimizer currently doesn’t help much; it only removes assert statements. When -O is used, all bytecode is optimized; .pyc files are ignored and .py files are compiled to optimized bytecode.
  - Passing two -O flags to the Python interpreter (-OO) will cause the bytecode compiler to perform optimizations that could in some rare cases result in malfunctioning programs. Currently only __doc__ strings are removed from the bytecode, resulting in more compact .pyo files. Since some programs may rely on having these available, you should only use this option if you know what you’re doing.
  - A program doesn’t run any faster when it is read from a .pyc or .pyo file than when it is read from a .py file; the only thing that’s faster about .pyc or .pyo files is the speed with which they are loaded.
  - When a script is run by giving its name on the command line, the bytecode for the script is never written to a .pyc or .pyo file. Thus, the startup time of a script may be reduced by moving most of its code to a module and having a small bootstrap script that imports that module. It is also possible to name a .pyc or .pyo file directly on the command line.
  -It is possible to have a file called spam.pyc (or spam.pyo when -O is used) without a file spam.py for the same module. This can be used to distribute a library of Python code in a form that is moderately hard to reverse engineer.
  - The module compileall can create .pyc files (or .pyo files when -O is used) for all modules in a directory.
* `dir()` function
* packages
  - `__init__.py` files are required to make Python treat the directories as containing packages; 
  - `__init__.py` can just be an empty file, but it can also execute initialization code for the package or set the `__all__` variable, described later.
  - `__int__.py` will call exactly once as `import package` or `import package.submodule` called
  ```
  import package
  import package.module1
  import package.module2
  # will only call __init__.py once
  ```
* import searching
  - module cache, `sys.modules`
  - import path
  - import hooks, *meta hooks and import path hooks.*
  - mate path,  Meta path finders must implement a method called find_spec() which takes three arguments: a name, an import path, and (optionally) a target module. The meta path finder can use any strategy it wants to determine whether it can handle the named module or not.
* module loaders
  - Module loaders provide the critical function of loading: module execution. 
  - submodules, When a submodule is loaded using any mechanism (e.g. importlib APIs, the import or import-from statements, or built-in __import__()) a binding is placed in the parent module’s namespace to the submodule object.

* Path entry finders, responsible for finding and loading Python modules and packages whose location is specified with a string path entry. Most path entries name locations in the file system, but they need not be limited to this.
  - The path based finder is a meta path finder, so the import machinery begins the import path search by calling the path based finder’s find_spec() method as described previously. 

* [The import system](https://docs.python.org/3/reference/import.html)
* [Modules](https://docs.python.org/2/tutorial/modules.html)
* [What is __init__.py for?](https://stackoverflow.com/questions/448271/what-is-init-py-for)

## Disassembler for Python bytecode
```python
def func(alist):
  return len(alist)
  
import dis
dis.dis(func)
 2           0 LOAD_GLOBAL              0 (len)
              3 LOAD_FAST                0 (alist)
              6 CALL_FUNCTION            1
              9 RETURN_VALUE
```
* [32.12. dis — Disassembler for Python bytecode](https://docs.python.org/3/library/dis.html)

## Python C Extensions
1. write a c extension
```C
static PyObject* method(PyObject* self, PyObject* args) {
  ...
  return Py_None; //or a python object or something else
}

static PyMethodDef method_defs[] = {
  {"method_name", method_name, METH_NOARGS, "method descript"},
  {NULL, NULL, 0, NULL}
};

static struct PyModuleDef moduleDef = {
  PyModuleDef_HEAD_INIT,
  "module_name",
  "module description",
  -1,
  method_defs
};

PyMODINIT_FUNC PyInit_module_name(void) {
  return PyModule_Create(&moduleDef)
}
```
2. add setup.py
```python
from distutils.core import setup, Extension
setup(name='module_name', version="1.0", \
  ext_modules = [Extension('module_name', ['extension.c'])])
```
3. build and install
```shell
python setup.py build
python setup.py install
```
* [Creating Basic Python C Extensions - Tutorial](https://tutorialedge.net/python/python-c-extensions-tutorial/#the-basic-requirements)
* [Python - Extension Programming with C](https://www.tutorialspoint.com/python/python_further_extensions.htm)

## Python Debug
### PDB
* `import pdb; pdb.set_trace()`
* `breakpoint()` # starting in python3.7
* `python -m pdb some.py`

* [Python Debugging With Pdb](https://realpython.com/python-debugging-pdb/)
* [pdb — The Python Debugger](https://docs.python.org/3/library/pdb.html)

### Debug with GDB or LLDB
* gdb
  - GDB 7+ has built-in python intergrations
  - debugging symbols
  ```shell
  gcc -g #compile python with debug info
  #ubuntu
  sudo apt install python-dbg libpython-dbg
  #centos
  yum instlal python-debuginfo
  ```
  - For processes that aren't your descendants
    - sysctl kern.global-ptrace=1 (OpenBSD)
    - sysctl kernel.yama.ptrace_scopre=0 (Linux) (if you have a YAMA-enabled kernel)
    - Don't enable these in production, they have serious security implications!
  - debug
  ```shell
  gdb --args python <program>.py <arguments>
  # or
  gdb python
  ...
  (gdb) run <program>.py <arguments>
  # or if the process is already running
  gdb python <pid of running process>
  ```
  - commands
  `py-bt, py-list` etc.
  
* [DebuggingWithGdb](https://wiki.python.org/moin/DebuggingWithGdb)
* [Debugging of CPython processes with gdb](https://www.podoliaka.org/2016/04/10/debugging-cpython-gdb/)
* [22. gdb Support](https://devguide.python.org/gdb/)
* [Debugging: stepping through Python script using gdb?](https://stackoverflow.com/questions/7412708/debugging-stepping-through-python-script-using-gdb)
* lldb
```
lldb
(lldb)attache --pid some_python_pid
(lldb)continue
(lldb)breakpoint set -f python_c_extension.c -l codeline
```
* [Debugging C/C++ libraries called by Python](http://johntfoster.github.io/posts/debugging-cc%2B%2B-libraries-called-by-python.html)
