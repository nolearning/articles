# DB Notes

## tips
### using SELECT in the WHERE clause
```sql
SELECT * FROM Individual
INNER JOIN Publisher
ON Individual.IndividualId = Publisher.IndividualId
WHERE Individual.IndividualId = /* in */ (SELECT someID FROM table WHERE blahblahblah)
```
* solution to *ERROR 1242 (21000): Subquery returns more than 1 row*
  - return on row by using aggregation function
  - using exists
  
* [Using Subqueries in the WHERE Clause](https://www.essentialsql.com/get-ready-to-learn-sql-server-21-using-subqueries-in-the-where-clause/)

### multiple string intersection
```sql
--- wanna custom_find_in_set('a,b,c', 'a,b,c,d'), equal to find_in_set('a', 'a,b,c,d') OR find_in_set('b', 'a,b,c,d') OR find_in_set('b', 'a,b,c,d')

WHERE CONCAT(",", `setcolumn`, ",") REGEXP ",(val1|val2|val3),"
```

------------
* [MySQL find_in_set with multiple search string](https://stackoverflow.com/questions/5015403/mysql-find-in-set-with-multiple-search-string)
* [How to get the intersection of two columns](How to get the intersection of two columns)

### intersection of two arrays
```sql
--- prosgresql
SELECT ARRAY
    (
        SELECT UNNEST(a1)
        INTERSECT
        SELECT UNNEST(a2)
    )
FROM  (
        SELECT  array['two', 'four', 'six'] AS a1
              , array['four', 'six', 'eight'] AS a2
      ) q;
      
--- posgresql for jsonb
select '["Category A", "Category D"]'::jsonb ?| array['Category A', 'Category B'];
```
* [Postgres - Function to return the intersection of 2 ARRAYs?](https://stackoverflow.com/questions/756871/postgres-function-to-return-the-intersection-of-2-arrays)
* [PostgreSQL json array intersection query](https://stackoverflow.com/questions/32812526/postgresql-json-array-intersection-query)

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
