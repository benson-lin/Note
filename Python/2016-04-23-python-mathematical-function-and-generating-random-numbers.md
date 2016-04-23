# Python Mathematical Function & Generating Random numbers


## Python Mathematical Function

Python有很多内置的函数，不用引入模块
```python
METHOD                   DESCRIPTION
round(number[, ndigits]) 四舍五入，第二个参数指定位数
pow(a, b)	            计算a^b
abs(x)	                返回x的绝对值
max(x1, x2, …, xn)	    返回最大值
min(x1, x2, …, xn)	    返回最小值
```

而下面的函数是在 'math'模块中的，所以在使用数学函数前记得导入模块 `import math`

```python
METHOD	    DESCRIPTION
ceil(x)	    向上取整(取大于等于x的第一个整数)
floor(x)	向下取整(取小于等于x的第一个整数)
sqrt(x)	    开平方
sin(x)	    求sin
cos(x)	    求cos
tan(x)	    求tan
```

```python
>>> abs(-22)               # Returns the absolute value
22
>>> max(9, 3, 12, 81)      # Returns the maximum number
81
>>> min(78, 99, 12, 32)    # Returns the minimum number
12
>>> pow(8, 2)              # can also be written as 8 ** 2
64
>>> pow(4.1, 3.2)          # can also be written as 4.1 ** 3.2
91.39203368671122
>>> round(5.32)            # Rounds to its nearest integer
5
>>> round(3.1456875712, 3) # Return number with 3 digits after decimal point
3.146

>>> import math
>>> math.ceil(3.4123)
4
>>> math.floor(24.99231)
24
In next post we will lear
```

## Python Generating Random numbers

产生随机数需要使用 `random` 模块

`random()` 默认产生一个 0<=x<1.0的随机数

而如果使用`randint(a,b)`，而产生a<=x<=b的整数，注意是包含左右边界的


```python
>>> import random
>>> for i in range(0, 10):
... print(random.random())


>>> for i in range(0, 10):
... print(random.randint(1, 10))
```

注意：如果要获取帮助信息，需要记得先引入模块，再使用help命令，否则会出现 `NameError: name 'XX' is not defined`的错误