# DB Notes

## Dumping and importing
### Mysql dump or import in an UTF-8
```shell
# not safe, it might screw up encoding
mysqldump -uroot -p database > sql.dump 

# better
mysqldump -uroot -p database -r sql.dump

# once mysql server is not set to UTF-8, e.g. latin, then remove SET NAMES='latin1' comment at the top of the dump,
# so the target machine won't change its UTF-8 charset when sourcing.
 mysqldump --default-character-set=latin1 -uroot -p database -r sql.dump

# only dump the structrue
mysqldump -uroot -p --no-data database -r utf8.dump

# Do not do this, since it might screw up encoding
mysql -u username -p database < dump_file # this is bad

# better
mysql -uroot -p --default-character-set=utf8 database
mysql> SET names 'utf8'
mysql> SOURCE utf8.dump
```
* [Dumping and importing from/to MySQL in an UTF-8 safe way](https://makandracards.com/makandra/595-dumping-and-importing-from-to-mysql-in-an-utf-8-safe-way)
