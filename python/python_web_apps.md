# Python Web Apps

## Werkkkzeug
* [Werkzeug 教程](https://werkzeug-docs-cn.readthedocs.io/zh_CN/latest/tutorial.html)

## Flask
* [Flask源码剖析](https://zhuanlan.zhihu.com/p/24629677)

## sqlalchemy

* [SQL Expression Language Tutorial](https://docs.sqlalchemy.org/en/latest/core/tutorial.html)
* [Object Relational Tutorial](https://docs.sqlalchemy.org/en/latest/orm/tutorial.html)
* [SQLAlchemy in Flask](http://flask.pocoo.org/docs/1.0/patterns/sqlalchemy/)

### engine, connection and session

* [SQLAlchemy: engine, connection and session difference](https://stackoverflow.com/questions/34322471/sqlalchemy-engine-connection-and-session-difference)
* [Understanding Python SQLAlchemy’s Session](https://www.pythoncentral.io/understanding-python-sqlalchemy-session/)
* [Working with Engines and Connections](https://docs.sqlalchemy.org/en/latest/core/connections.html)

#### session in concurrency
* Session objects are not thread-safe
* The `ScopeSession` is thread safe

* [Contextual/Thread-local Sessions](https://docs.sqlalchemy.org/en/latest/orm/contextual.html)


### raw sql
* [Using raw SQL with Flask-SQLAlchemy](http://neekey.net/2017/05/19/using-raw-sql-with-flask-sqlalchemy/)
* [Raw SQL](http://zetcode.com/db/sqlalchemy/rawsql/)
* [sqlalchemy : executing raw sql with parameter bindings](https://stackoverflow.com/questions/23206562/sqlalchemy-executing-raw-sql-with-parameter-bindings)
* [How to Execute Raw SQL in SQLAlchemy](https://chartio.com/resources/tutorials/how-to-execute-raw-sql-in-sqlalchemy/)

### ORM
#### whereclause
In sqlalchemy, when you call orm_model.select(orm_model.c.column == value), will pass a `sqlalchemy.sql.elements.BinaryExpression object`. 

Why?  
1. orm_model.c.column is `<class 'sqlalchemy.sql.schema.Column'>` instance
2. `<class 'sqlalchemy.sql.schema.Column'>` parent classes are `(<class 'sqlalchemy.sql.schema.SchemaItem'>, <class 'sqlalchemy.sql.elements.ColumnClause'>)`
3. `<class 'sqlalchemy.sql.elements.ColumnClause'>)` parent classes are `(<class 'sqlalchemy.sql.base.Immutable'>, <class 'sqlalchemy.sql.elements.ColumnElement'>)`
4. `<class 'sqlalchemy.sql.elements.ColumnElement'>)` parent classes are `(<class 'sqlalchemy.sql.operators.ColumnOperators'>, <class 'sqlalchemy.sql.elements.ClauseElement'>)`
5. `<class 'sqlalchemy.sql.operators.ColumnOperators'>` has operator overloadings, in this case is `ColumnOperators.__eq__`
6. so `orm_model.c.column == value` will call `ColumnOperators.__eq__` and return `sqlalchemy.sql.elements.BinaryExpression object`. 

--------------
* [Column Elements and Expressions](https://docs.sqlalchemy.org/en/latest/core/sqlelement.html)
* [sqlalchemy/lib/sqlalchemy/sql/elements.py](https://github.com/zzzeek/sqlalchemy/blob/master/lib/sqlalchemy/sql/elements.py)
* [sqlalchemy/lib/sqlalchemy/sql/schema.py](https://github.com/zzzeek/sqlalchemy/blob/master/lib/sqlalchemy/sql/schema.py)
* [sqlalchemy/lib/sqlalchemy/sql/operators.py](https://github.com/zzzeek/sqlalchemy/blob/master/lib/sqlalchemy/sql/operators.py)
### tips
```python
# show commands SQLAlchemy is sending
engine = create_engine('dialect+driver://...', echo=True)

```
-----------

* [Why in SQLAlchemy base.metadata.create_all(engine) in python console not showing table?](https://stackoverflow.com/questions/23705948/why-in-sqlalchemy-base-metadata-create-allengine-in-python-console-not-showing)

## Deployment

0. Development Server, such as `Django Development Server`, `Flask Simple Server`
1. Standalone WSGI Containers
  - [Standalone WSGI Containers](http://flask.pocoo.org/docs/1.0/deploying/wsgi-standalone/)
2. mod_wsgi(Apache)
  - install mod_wsgi
  - creat .wsgi file
  ```python
  from myWebApp import app as application
  ```
  - configure apache
  ```xml
  <VirtualHost entry>
    WSGIScriptAlias /myWebApp /opt/myenv/myWebApp.wsgi
    <Directory /path/to/myWebApp>
      Order allow,deny
      Allow from all
    </Directory>
  </VirtualHost>
  ```
  - [mod_wsgi (Apache)](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/#mod-wsgi-apache)
  - [Minimal Apache configuration for deploying a flask app (Ubuntu 18.04)](https://www.codementor.io/abhishake/minimal-apache-configuration-for-deploying-a-flask-app-ubuntu-18-04-phu50a7ft)
3. uWSGI
  - [uWSGI](http://flask.pocoo.org/docs/1.0/deploying/uwsgi/)
  - [The uWSGI project](https://uwsgi-docs.readthedocs.io/en/latest/index.html)
  - [uWSGI vs. Gunicorn, or How to Make Python Go Faster than Node](https://blog.kgriffs.com/2012/12/18/uwsgi-vs-gunicorn-vs-node-benchmarks.html)
  - [How To Serve Flask Applications with uWSGI and Nginx on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uswgi-and-nginx-on-ubuntu-18-04)
  - [Django under the hood: django at instagram - Carl Meyer](https://reinout.vanrees.org/weblog/2016/11/04/instagram.html)
4. Gunicorn
  - [Deploy flask app with nginx using gunicorn and supervisor](https://medium.com/ymedialabs-innovation/deploy-flask-app-with-nginx-using-gunicorn-and-supervisor-d7a93aa07c18)
  
5. FastCGI
  - create a .fcgi file
  ```python
  #!/usr/bin/python
  from flup.server.fcgi import WSGIServer
  from yourapplication import app

  if __name__ == '__main__':
      WSGIServer(app).run()
  ```
  - configure Apache or Nginx
  - run FastCGI app
  - [FastCGI](http://flask.pocoo.org/docs/1.0/deploying/fastcgi/)

* [Django Server Comparison: The Development Server, Mod_WSGI, uWSGI, and Gunicorn](https://www.digitalocean.com/community/tutorials/django-server-comparison-the-development-server-mod_wsgi-uwsgi-and-gunicorn)


## Reference
* [Maximizing Python Performance with NGINX, Part 1: Web Serving and Caching](https://www.nginx.com/blog/maximizing-python-performance-with-nginx-parti-web-serving-and-caching/)
