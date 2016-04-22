# Python Dictionaries


Python 中的字典用来保存键值对，允许你快速的根据key获取，添加，删除，修改元素；与其他语言的hash非常相似

注意：Dictionaries 是可变的

## Creating Dictionary

使用 `{}`

```python
>>> friends = {
... 'tom' : '111-222-333',
... 'jerry' : '666-33-111'
... }
>>> dict_emp = {}
```

## Retrieving, modifying and adding elements in the dictionary

语法：
```python
# 获取元素
>>> dictionary_name['key']
# 添加或修改元素
>>> dictionary_name['newkey'] = 'newvalue'
# 删除元素
>>> del dictionary_name['key']
```

## Looping items in the dictionary

循环获取value值

```python
>>> for key in friends:
...     print(key,":",friends[key])
...
tom : 111-222-333
jerry : 666-33-111
```

## Dictionary Common Operations

`len()`, `in`, `not in`(检查key，而不是value)

```python
>>> len(friends)
2
>>> 'tom' in friends
True
>>> 'tom' not in friends
False
```

## Equality Tests in dictionary

使用`==`, `!=`判断两个 dictionary 是否相等，只要包含相同的key和value，不要求顺序；
不能使用 `<`, `>`, `>=`,  `<=`比较dictionaries

```python
>>> d1 = {"mike":41, "bob":3}
>>> d2 = {"bob":3, "mike":41}
>>> d1 == d2
True
>>> d1 != d2
False
```

## Dictionary methods

```python
METHODS	       DESCRIPTION
popitem()	   随机获取一个元素，并删除，如果不存在元素出现KeyError
clear()	       清空字典的内容，字典对象仍然存在
keys()         返回所有的key的tuples(即不可变的元组，()作为边界)
values()	   返回所有的value的tuples(下一节中讨论)
get(key)	   获取某个key的值，不存在返回None
pop(key)	   删除某个key值元素，并返回对应的value值，如果不存在会出现KeyError
```