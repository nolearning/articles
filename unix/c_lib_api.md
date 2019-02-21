# C Lib API

* [stat](https://linux.die.net/man/3/stat)
* [getpwuid](https://linux.die.net/man/3/getpwuid)

## Get File Owner's Name
```c
#include <pwd.h>
#include <stdio.h>
#include <sys/stat.h>

int main() {
  const char* file = "stat_demo.c";
  struct stat fileStat;
  struct passwd *pw;
  if(stat(file, &fileStat) < 0) {
    return 1;
  }

  pw = getpwuid(fileStat.st_uid);
  if (pw) printf("%s\n", pw->pw_name);
}
```
