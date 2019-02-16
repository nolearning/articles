# Shell Notes
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
