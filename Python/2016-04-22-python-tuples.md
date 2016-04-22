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