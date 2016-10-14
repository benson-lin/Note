# Datatype & Variables

## Identifiers

我们为变量和函数起的名字被公认为是标识符，用来表示对象在内存中的的引用

下面的几点Python标识符的规则和其他语言的规则类似：

1. 所有标识符必须以字母或下划线开头，不能使用数字
2. 标识符可以包含字母，数字和下划线
3. 可以任意长度
4. 标识符不能是关键字

下面是 python3 的关键字

```python
False      class      finally    is         return
None       continue   for        lambda     try
True       def        from       nonlocal   while
and        del        global     not        with
as         elif       if         or         yield
pass       else       import     assert
break      except     in         raise
```

## Assigning Values to Variables

在 python 中，**任何东西都是对象，不管是不是基本数据类型**；像 int,float,string 都是对象

python 中不必提前声明变量类型，解释器会根据数据自动检查变量的类型

```python
x = 100                       # x is integer
pi = 3.14                     # pi is float
empname = "python is great"   # empname is string
```

上面的例子中，**x 存储了 100（是一个对象）的引用，x 不保存 100 本身**


## Simultaneous Assignments

python 允许同时赋值的语法，如下

```python
var1, var2, ..., varn = exp1, exp2, ..., expn
```

同时赋值对交换两个变量有帮助
```python
>>> x = 1
>>> y = 2
 
>>> y, x = x, y # assign y value to x and x value to y
```
输入
```python
>>> print(x)
2
>>> print(y)
1
```


## Python Data Types

Python 有5种标准的数据类型(具体的在下面的其他文章中)：

- Numbers(数字)
- String(字符串)
- List(列表)
- Tuple(不可变元组)
- Dictionary(key-value形式，类比Java中的HashMap)
- Boolean

Python中 True 和 False 是布尔常量，下面的几种情况会被认为是 False

1. 0, 0.0 – zero , 
2. [] – empty list 
3. () – empty tuple 
4. {} – empty dictionary 
5. "" - empty string
6. None

## Receiving input from Console

input()函数用于从控制台接受输入

语法：`input([prompt]) -> string`

接受一个叫做"提示"的可选的字符串参数，返回一个字符串；
```python
>>> name = input("Enter your name: ")
>>> Enter your name: tim
>>> name
'tim'
>>> type(name)
<class 'str'>
```
即使你输入一个数字也会返回字符串，因此需要用int()或者eval()进行转换
```python
>> age = int(input("Enter your age: "))
Enter your age: 22
>>> age
22
>>> type(age)
<class 'int'>
```

## Importing modules

Python通过模块组织代码。Python 提供了很多内置的模块给我们使用，比如math模块用于数学相关的函数，re模块提供正则表达式的判断；使用前需要 import 模块

语法： `import module_name_1, module_name_2`

```python
>>> import math
>>> math.pi
3.141592653589793
```

import 引入了所有在math中的所有的函数，类，变量和常量。为了访问math模块中的内容，使用 `module_name.variable`（或者`module_name.class`, `module_name.function`,`module_name.constant`）


