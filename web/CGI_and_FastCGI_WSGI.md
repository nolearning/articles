# CGI and FastCGI

## CGI
* CGI is a protocol for interfacing external applications to web servers
* CGI applications run in separate processes, which are created at the start of each request and torn down at the end

* [Common Gateway Interface wikipedia](https://en.wikipedia.org/wiki/Common_Gateway_Interface)
* [Getting Started with CGI Programming in C](http://jkorpela.fi/forms/cgic.html)

## FastCGI
* FastCGI uses persistent processes to handle a series of requests
* To service an incoming request, the web server sends environment information and the page request itself to a FastCGI process over either a Unix domain socket, a named pipe or a TCP connection. Responses are returned from the process to the web server over the same connection, and the web server subsequently delivers that response to the end-user. The connection may be closed at the end of a response, but both the web server and the FastCGI service processes persist.
* Each individual FastCGI process can handle many requests over its lifetime, thereby avoiding the overhead of per-request process creation and termination. 

* [FastCGI wikipedia](https://en.wikipedia.org/wiki/FastCGI)

## WSGI
* The Web Server Gateway Interface (WSGI) is a simple calling convention for web servers to forward requests to web applications or frameworks written in the Python programming language.
* [Web Server Gateway Interface wikipedia](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface)
* [PEP 3333 -- Python Web Server Gateway Interface v1.0.1](https://www.python.org/dev/peps/pep-3333/)
