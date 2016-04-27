# Python *args and **kwargs


## What is *args ??

`*args` 允许我们传递多个数值参数给函数

首先我们创建一个函数累加两个数的和
```python
def sum(a, b):
    print("sum is", a+b)
```
如果要接受多个参数进行累加，类似Java，C等都是传递数组作为参数，Python也不例外，这时就需要用`*args`,对应数据类型的 List

```python
def sum(*args):
    s = 0
    for i in args:
        s += i
    print("sum is", s)
```

此时，可以传递任意多的参数到函数中，即使不填也可以，学过 Java 的可以类比 Java 中的形参为 `object..`的形式。


使用如下：
```python
>>> sum(1, 2, 3)
6
>>> sum(1, 2, 3, 4, 5, 7)
>>> sum()
0
```



## What is **kwargs ?

`**kwargs`允许我们传递参数如`func_name(name='tim', team='school')`的形式，对应数据类型的Dictionary

下面列出遍历方式


```python
def my_func(**kwargs):
    for i, j in kwargs.items():
        print(i, j)
 
my_func(name='tim', sport='football', roll=19)
```

输出
```python
sport football
roll 19
name tim
```

## Using *args and **kwargs in function call

在调用函数时使用`*args`和`**kwargs`意味着


如何在实参中传递一个 List？
```python
def my_three(a, b, c):
    print(a, b, c)
 
a = [1,2,3]
my_three(*a) # here list is broken into three elements
```
要成功传递一个 List 需要注意一点
1. List 的参数个数要和函数参数个数相同


如何传递一个 Dictionary？
```python
def my_three(a, b, c):
    print(a, b, c)
 
a = {'a': "one", 'b': "two", 'c': "three" }
my_three(**a)
```

要成功传递一个 Dictionary 需要注意两点：
1. 函数的参数名必须与Dictionary中的key的值一致
2. 函数参数个数应该和Key的个数一致

## Combining them 

如果要同时在调用函数的实参和函数的形参使用`*args`，可以这么做

```python
def two(*list):
    for i in list:
        print(i)
 
a = [1,3,4]
two(*a)
```