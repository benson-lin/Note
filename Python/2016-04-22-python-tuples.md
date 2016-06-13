# Python Tuples

Python 中的 Tuples(元组) 与 List 非常相似，但是一旦 tuple被创建，将不能添加，删除，替换和排序元素

学会使用help(tuple)命令,由于所有的操作和前面的其他数据类型非常相似，所以只简单的写上代码

注意：Tuples 是不可变的


## Creating a tuple

```python
>>> t1 = () # creates an empty tuple with no data
>>> t2 = (11,22,33)
>>> t3 = tuple([1,2,3,4,4]) # tuple from array
>>> t4 = tuple("abc") # tuple from string 
# 看到有书说：如果要构造只有一个值的元组，至少需要一个逗号，否则报错，我测试过是没问题的
>>> t6 = (34)
>>> t6
34
```

## Tuples functions

`max`, `min`, `len`, `sum`函数也能用在元组中

```python
>>> t1 = (1, 12, 55, 12, 81)
>>> min(t1)
1
>>> max(t1)
81
>>> sum(t1)
161
>>> len(t1)
5
```

## Iterating through tuples

```python
>>> t = (11,22,33,44,55)
>>> for i in t:
... print(i, end=" ")
>>> 11 22 33 44 55
```

## Slicing tuples

```python
>>> t[0:2]
(11,22)
```

## in  and not in  operator

```python
>>> t = (11,22,33,44,55)
>>> 22 in t
True
>>> 22 not in t
False
```

## What is the meaning of tuple

元组有什么用呢？为什么不能只使用列表？因为元组有不可替代的作用：
- 元组可以在Dictionary数据结构中当键使用，而列表不行(Dictionary一节有示例)
- 元组作为很多内建函数和方法的返回值，此时你只能对元组进行处理，而不能修改元组