#GDB Notes

## Symbol
```
# compile with debug infomation
gcc -g -o main main.c

# separate the debug infomation
objcopy --only-keep-debug main main.debug
or
cp main main.debug
strip --only-keep-debug main.debug

# strip debug information from origin file
objcopy --strip-debug main
or
strip --strip-debug --strip-unneeded main

# debug by debuglink mode:
objcopy --add-gnu-debuglink main.debug main
gdb main

# use exec file and symbol file separatly
gdb -s main.debug -e main
or
gdb
(gdb) exec-file main
(gdb) symbol-file main.debug
```
* [How gdb loads symbol files](https://sourceware.org/gdb/wiki/How%20gdb%20loads%20symbol%20files)
* [How to load multiple symbol files in gdb](https://stackoverflow.com/questions/20380204/how-to-load-multiple-symbol-files-in-gdb)
* [How to generate gcc debug symbol outside the build target?](https://stackoverflow.com/questions/866721/how-to-generate-gcc-debug-symbol-outside-the-build-target)

## coredump
* `ulimit -c unlimited` to allow core creation
* `sudo sysctl -w kernel.core_pattern=/tmp/core-%e.%p.%h.%t` set coredump file pattern and location
----------------
* [How to get a core dump for a segfault on Linux](https://jvns.ca/blog/2018/04/28/debugging-a-segfault-on-linux/)
* [How to generate core dump file in Ubuntu [duplicate]](https://stackoverflow.com/questions/6152232/how-to-generate-core-dump-file-in-ubuntu)
* [How to generate a core dump in Linux on a segmentation fault?](https://stackoverflow.com/questions/17965/how-to-generate-a-core-dump-in-linux-on-a-segmentation-fault)
* [10.19 How to Produce a Core File from Your Program](https://sourceware.org/gdb/onlinedocs/gdb/Core-File-Generation.html)

## Python extension
* pretty-printer
```python
class fooPrinter:
  """Print a foo object."""

  def __init__(self, val):
    self.val = val

  def to_string(self):
    return ("a=<" + str(self.val["a"]) +
            "> b=<" + str(self.val["b"]) + ">")

class barPrinter:
  """Print a bar object."""

  def __init__(self, val):
    self.val = val

  def to_string(self):
    return ("x=<" + str(self.val["x"]) +
            "> y=<" + str(self.val["y"]) + ">")
import gdb.printing

def build_pretty_printer():
  pp = gdb.printing.RegexpCollectionPrettyPrinter(
    "my_library")
  pp.add_printer('foo', '^foo$', fooPrinter)
  pp.add_printer('bar', '^bar$', barPrinter)
  return pp

import gdb.printing
import my_library
gdb.printing.register_pretty_printer(
  gdb.current_objfile(),
  my_library.build_pretty_printer())
```
-------------
* [CppCon 2016: Greg Law “GDB - A Lot More Than You Knew"](https://www.youtube.com/watch?v=-n9Fkq1e6sg)
* [23.2.2.7 Writing a Pretty-Printer](https://sourceware.org/gdb/onlinedocs/gdb/Writing-a-Pretty_002dPrinter.html)
