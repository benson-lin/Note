# Python Functions

函数是能够被复用的代码片段，有助于组织代码的结构。

## Creating functions
Python 有自己的关键字 'def' 作为函数的开始；

语法：
```python
def function_name(arg1, arg2, arg3, .... argN):
     #statement inside function
```
注意：所有在函数中的语句，要保持相同的空格缩进

可以使用 `pass` 关键字忽略函数体
```python
def myfunc():
    pass
```

例子：
```python
def sum(start, end):
   result = 0
   for i in range(start, end + 1):
       result += i
   print(result)
 
sum(10, 50)
```

range()函数的用途，通过查看`help`命令可以了解：
```python
class range(object)
 |  range(stop) -> range object
 |  range(start, stop[, step]) -> range object
 |
 |  Return an object that produces a sequence of integers from start (inclusive)
 |  to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
 |  start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
 |  These are exactly the valid indices for a list of 4 elements.
 |  When step is given, it specifies the increment (or decrement).
```

## Function with return value.

带返回值的函数；
上面的例子中同时用 `print` 将结果打印在控制台，如果想将一个变量返回，需要使用 `return` 语句将结果返回给调用者并退出函数

如上面的例子中
```python
def sum(start, end):
   result = 0
   for i in range(start, end + 1):
       result += i
   return result
 
s = sum(10, 50)
print(s)
```

你可以使用直接使用 `return`而不返回任何值，此时返回值为None

```python
def sum(start, end):
   if(start > end):
       print("start should be less than end")
       return    # here we are not returning any value so a special value None is returned
   result = 0
   for i in range(start, end + 1):
       result += i
   return result
 
s = sum(110, 50)
print(s)
# 结果：
# start should be less than end
# None
```

如果不使用 `return`语句，也总是返回 None
```python
def test():   # test function with only one statement
    i = 100
print(test())
```

## Global variables vs local variables

全局变量：全局变量不受函数约束，按可以在函数外和函数内被访问
局部变量：声明在函数内的变量

```python
global_var = 12         # a global variable

def func():
    local_var = 100     # this is local variable
    print(global_var)   # you can access global variables in side function

func()                  # calling function func()

#print(local_var)        # you can't access local_var outside the function, because as soon as function ends local_var is destroyed
```

如果同时存在相同变量名的局部变量和全局变量，那么函数内的变量以局部变量为准，且不修改全局变量；下面是例子
```python
xy = 100
 
def cool():
    xy = 200     # xy inside the function is totally different from xy outside the function
    print(xy)    # this will print local xy variable i.e 200
 
cool()
 
print(xy)        # this will print global xy variable i.e 100
```
结果：
```python
200
100
```

如果确实需要将局部变量扩展到全局范围内，可以使用`global`关键字，此时局部变量也是全部变量

```python
t = 1
 
def increment():
    global t   # now t inside the function is same as t outside the function
    t = t + 1
    print(t) # Displays 2
 
 
increment()
print(t) # Displays 2
```
结果：
```python
2
2
```
注意：不能为一个加了`global`关键字的变量赋值；如下

```python
t = 1
 
def increment():
    #global t = 1   # this is error，错误
    global t
    t = 100        # this is okay，正确
    t = t + 1
    print(t) # Displays 101
 
 
increment()
print(t) # Displays 101
```

事实上，不需要将全部变量声明在函数外，完全可以将全局变量在函数内声明

```python
def foo():
    global x   # x is declared as global so it is available outside the function 
    x = 100
 
foo()
print(x) #可以访问到x变量
```


## Argument with default values

如何为函数的参数赋予默认值

```python
def func(i, j = 100):
    print(i, j)
func(2) # here no value is passed to j, so default value will be used
```
可以在形参中指明初始值，在调用函数的时候不传递该参数，将使用默认值

## Keyword arguments

有两种参数可以为方法传递参数：
- 一种是位置型参数(positional arguments)，即严格按照形参本身的顺序赋值，也就是其他语言调用参数的形式
- 一种是关键字参数(Keyword arguments)，可以通过`name=value`形式传递每一个参数，这样就不必关心参数的顺序；例子如下

```python
def named_args(name, greeting):
    print(greeting + " " + name )
    
named_args(name='jim', greeting='Hello')
Hello jim

named_args(greeting='Hello', name='jim') # you can pass arguments this way too
Hello jim
```

## Mixing Positional and Keyword Arguments

有可能会出现混合上面的两种传参方式，但是位置型参数一定是出现在关键字参数前面

```python
def my_func(a, b, c):
    print(a, b, c)
```
你能够用下面几种方式调用函数：
```python
# using positional arguments only
my_func(12, 13, 14)     
 
# here first argument is passed as positional arguments while other two as keyword argument
my_func(12, b=13, c=14) 
 
# same as above
my_func(12, c=13, b=14) 
 
# this is wrong as positional argument must appear before any keyword argument 
# my_func(12, b=13, 14)
```

## Returning multiple values from Function

Python可以返回多个参数，这个有点意思，其他编程语言一般都不可以的。当有多个返回值时，返回的类型为tuples，即元组类型，不可变，但具有 List 的特点。具体可以查看前面的数据类型介绍。

```python
def bigger(a, b):
    if a > b:
        return a, b
    else:
        return b, a

s = bigger(12, 100)
print(s)
print(type(s))
# (100, 12)
# <class 'tuple'>
```







