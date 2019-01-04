# MVCC (Multi-Version Concurrency Control)

## Concurrency Control theory
* pessimistic lock
  - Read/Write lock
  - Two-Phrase lock
* optimistic lock
  - logical clock
  - MVCC


## Reference
* [How does MVCC (Multi-Version Concurrency Control) work](https://vladmihalcea.com/how-does-mvcc-multi-version-concurrency-control-work/)
* [How MVCC databases work internally](https://medium.com/@kousiknath/how-mvcc-databases-work-internally-84a27a380283)
* [InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)
* [Understanding MySQL Isolation Levels: Repeatable-read](https://blog.pythian.com/understanding-mysql-isolation-levels-repeatable-read/)

* [PosgreSQL Concurrency Control](https://www.postgresql.org/docs/11/mvcc.html)
* [PostgreSQL Transaction Isolation READ UNCOMMITTED](https://stackoverflow.com/questions/33646012/postgresql-transaction-isolation-read-uncommitted)
* [Understanding PSQL's MVCC](http://eric.themoritzfamily.com/understanding-psqls-mvcc.html)
* [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
* [MVCC and VACUUM](http://rhaas.blogspot.com/2017/12/mvcc-and-vacuum.html)
* [PostgreSQL anti-patterns: read-modify-write cycles](https://blog.2ndquadrant.com/postgresql-anti-patterns-read-modify-write-cycles/)

* [Wikipedia Multiversion concurrency control](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)
* [Wikipedia Serializability](https://en.wikipedia.org/wiki/Serializability)
* [Wikipedia Snapshot isolation](https://en.wikipedia.org/wiki/Snapshot_isolation#Serializable_Snapshot_Isolation)
* [Wikipedia Isolation](https://en.wikipedia.org/wiki/Isolation_(database_systems)

* [Implementing Your Own Transactions with MVCC](http://elliot.land/post/implementing-your-own-transactions-with-mvcc)
* [Implementation of MVCC Transactions for Key-Value Stores](https://highlyscalable.wordpress.com/2012/01/07/mvcc-transactions-key-value/)
* [Distributed MVCC And Transactional SQL Design](https://cwiki.apache.org/confluence/display/IGNITE/Distributed+MVCC+And+Transactional+SQL+Design)
* [Apache HBase Internals: Locking and Multiversion Concurrency Control](https://blogs.apache.org/hbase/entry/apache_hbase_internals_locking_and)

* [Why does MVCC require locking for DML statements](https://stackoverflow.com/questions/30546187/why-does-mvcc-require-locking-for-dml-statements)
  * first committer win
  * first updater win
  

* [数据库中的引擎、事务、锁、MVCC（三）](https://zhuanlan.zhihu.com/p/53921376)
