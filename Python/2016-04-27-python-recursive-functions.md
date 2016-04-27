# Python recursive functions


python的递归函数基本和其它编程语言没什么区别

从下面的例子中可以看到

```python
# 求阶乘
def fact(n):
    if n == 0:
        return 1
    else:
        return n * fact(n-1)
 
 
print(fact(0))
print(fact(5))
```

注意：如果使用`print(fact(2000))`，将会得到错误:`RuntimeError: maximum recursion depth exceeded in comparison`

这是因为python默认会阻止递归层次超过1000次，因此，如果希望超过1000层，则可以使用下面的方式

```python
import sys
sys.setrecursionlimit(3000)
 
def fact(n):
    if n == 0:
        return 1
    else:
        return n * fact(n-1)
 
 
print(fact(2000))
```






