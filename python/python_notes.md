# Python Notes
## Install
* Mac OSX
  - `brew install python` will install python3
  - `brew link/unlink python` 
  - `brew info python`
  - python3 will be `/usr/local/bin/python3`, python will be python2, because some application in OSX may depend on python2
  - [How to set Python's default version to 3.x on OS X?](https://stackoverflow.com/questions/18425379/how-to-set-pythons-default-version-to-3-x-on-os-x/18425592)
* virtualenv
  - `pip install virtualenv`
  - `cd projectDir` && `virtualenv venv`  && `source venv/bin/activate`
  - `pip install <package>` under venv
  - `deactivate`
  
## Formatting
* % - old style
* '{}'.format - new style

* [Using % and .format() for great good!](https://pyformat.info/)

## Decorators
```python
def log(func):
  def decorator(*args, **kwargs):
    print('Log: function {} is calling'.format(func.__name__))
    return func(*args, **kwargs)
  return decorator

def tag(tagName):
  def tag_decorator(func):
    def decorator(*args, **kwargs):
      return '<{0}>{1}</{0}>'.format(tagName, func(*args, **kwargs))
    return decorator
  return tag_decorator

@tag('p')
@log
def greet(name):
  return 'Hello {}'.format(name)
```

* [Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)
* [A guide to Python's function decorators](https://www.thecodeship.com/patterns/guide-to-python-function-decorators/)
