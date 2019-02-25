# Python Tips

## create dict from array
```python
keys = ['a', 'b', 'c']
values = [1, 2, 3];
my_dict = dict(zip(keys, values))
```
  - [How to create Dict from array in python](https://stackoverflow.com/questions/9793603/how-to-create-dict-from-array-in-python)

## flatten a nested list mixed with list and numbers
```python
[num for item in nested for num in (item if isinstance(item, list) else (item,))]
```
  - [Writing a list comprehension to flatten a nested list [duplicate]](https://stackoverflow.com/questions/44417443/writing-a-list-comprehension-to-flatten-a-nested-list)
  - [flatten list of list through list comprehension](https://stackoverflow.com/questions/17338913/flatten-list-of-list-through-list-comprehension)

## string to datetime
```python
from datetime import datetime
datetime_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')
```

  - [Converting string into datetime](https://stackoverflow.com/questions/466345/converting-string-into-datetime)
  - [strftime() and strptime() Behavior](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior)

## accessing the index in 'for' loop
```python
for index, value in enumerate(collection):
  pass
```
* [Accessing the index in 'for' loops?](https://stackoverflow.com/questions/522563/accessing-the-index-in-for-loops)
