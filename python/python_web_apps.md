# Python Web Apps

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
