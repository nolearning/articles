# Mixin
## Two main situations where mixins are used

## Python
  Before 2.2, the implementation was: `depth first and then left to right`
```python
def classic_lookup(cls, name):
    "Look up name in cls and its base classes."
    if cls.__dict__.has_key(name):
        return cls.__dict__[name]
    for base in cls.__bases__:
        try:
            return classic_lookup(base, name)
        except AttributeError:
            pass
    raise AttributeError, name
```
so for a multiple inheritance
```
              class A:
                ^ ^  def save(self): ...
               /   \
              /     \
             /       \
            /         \
        class B     class C:
            ^         ^  def save(self): ...
             \       /
              \     /
               \   /
                \ /
              class D
```
the classic lookup rule visits the classes would be: `D B A C A`
``` python
d = new D()
d.save() // ==> call A.save not C.save
```
### C3 algorithm

* [The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/)
* [Python Method Resolution Order](https://medium.com/technology-nineleaps/python-method-resolution-order-4fd41d2fcc)

### Mixins and Python
[Mixins and Python](https://www.ianlewis.org/en/mixins-and-python)
