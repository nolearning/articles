# Bytecode And Assembly

## Bytecode
```shell
javap -v javaClassFile
```
* 
## Java Advanced Options

* []()

## hsdis plugin
```shell
# Get hsdis source code for current java version
hg clone http://https://hg.openjdk.java.net/jdkX/jdkX  # X will be 6, 7, 8, 9, 10
cd jdkX
source get_source
cd hotspot/src/share/tools/hsdis/
# or just download the source, e.g. jdk10
wget https://hg.openjdk.java.net/jdk10/jdk10/hotspot/archive/5ab7a67bc155.zip/src/share/tools/hsdis/
