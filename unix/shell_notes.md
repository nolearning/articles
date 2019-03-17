# Shell Notes
## shell script
```shell
cd . # dot stands for current directory
touch .hidden_dir # hidden directory's name start with dot
. shell_script # dot is alias for source, will run the shell script

cd .. # double dots stands for parent directory
# {..} make sequence
echo {1..10} # 1 - 10
echo {1..10..2} # 1 3 5 7 9
echo {10..1..2} # 10 8 6 4 2
echo {000..121..2} # 000 002 ... 120
echo {a..z}{a..z} # aa ab ... az ba bb ... zz
mkdir {2009..2019}_dir 
touch file_{a..z}.txt
touch {blahg,splurg,mmmf}_file.txt

# define array
months=("Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec")
echo ${month[3]} # get the forth element in array

# using {} and () to define all 8 digits binary
dec2bin=({0..1}{0..1}{0..1}{0..1}{0..1}{0..1}{0..1}{0..1})
echo ${dec2bin[25]} # 00011001

# ${...} parameter expension
str= "too longgg"
echo ${$str%gg} # expend str and delete characters after %
img=some.jpg
mv img ${img%jpg}png # change file's extension
echo another${img#some} # delete characters before #

echo '123'; echo '45' > text.txt # only '45' in text.txt
{echo '123'; echo '45'} > text.txt # '123\n45' in text.txt
```
* [Linux 工具：点的含义](https://linux.cn/article-10465-1.html)

## sed
```shell
# remove empty line
sed /^$/d file.txt # d: delete matched characters
```

## grep
```shell
# remove empty line
grep -Ev "^$" file.txt # -E --extended-regexp; -v --invert-match Selected lines not matching pattern
grep -v -e "^$" file.txt # -e pattern
```

## awk
```shell
awk NF file.txt
awk '!/^$/' file.txt # ! delete matched characters
awk "/./" file.txt
```

## tr
```shell
cat file.txt | tr -s '\n' -s  # squeeze multiple occurrences of the characters listed in the last operand
```
* [tr (Unix)](https://en.wikipedia.org/wiki/Tr_(Unix))

## tar
```shell
# compress
tar -cf tarfile.tar files
tar -rf appendto_tarfile.tar append_files
tar -uf update_tarfile.tar update_files
tar -xf extract_tarfile.tar
# -z will call gzip, -j will call bzip2
tar -czf tarfile.tar.gz files
tar -cjf tarfile.tar.bz2 files
# uncompress

# list the contents of a tar or tar.gz file
tar -ztvf tarfile.tar.gz
tar -tvf tarfile.tar[.gz|.bz2]
tar -jtvf tarfile.tar.bz2
tar -tvf tar.tar.gz 'search-pattern'
```

## References
* [在 Linux 中如何删除文件中的空行](https://zhuanlan.zhihu.com/p/59509755)
* [understanding the difference between “-C” and “-c” in tr(1) utility](https://unix.stackexchange.com/questions/74855/understanding-the-difference-between-c-and-c-in-tr1-utility)
