# Java Notes

## OpenJDK vs. Oracle JDK
Both OpenJDK and Oracle JDK are created and maintained currently by Oracle only.
* [Differences between Oracle JDK and OpenJDK](https://stackoverflow.com/questions/22358071/differences-between-oracle-jdk-and-openjdk)

## -XX Options
Here are three types of options that you can add to your JVM, standard, non-standard and advanced options. If you apply an advanced option you always precede the option with -XX:. Similarly if you’re using a non-standard option, you’ll use -X. Standard options don’t prepend anything to the option. 

Options that are specified with -XX are not stable and are subject to change without notice.

* Boolean options are turned on with -XX:+<option> and turned off with -XX:-<option>
* Numeric options are set with -XX:<option>=<number>
* String options are set with -XX:<option>=<string>, are usually used to specify a file, a path, or a list of commands

* [JVM Options Cheat Sheet](https://zeroturnaround.com/rebellabs/jvm-options-cheat-sheet/)
* [Java HotSpot VM Options](https://www.oracle.com/technetwork/articles/java/vmoptions-jsp-140102.html)
