# Python Exception Handling

异常处理让我们能够优雅的处理异常和做一些有意义的事，如输入错误信息等。

语法：
```python
try:
    # write some code 
    # that might throw exception
except <ExceptionType>: 
    # Exception handler, alert the user
```

例子：
```python
try:
    f = open('somefile.txt', 'r')
    print(f.read())
    f.close()
except IOError:
    print('file not found')
```

第一条语句一定会执行，如果没有异常将忽略`except`；
如果文件不存在`try`剩余的语句将不执行，进入except，`except`块将执行；

上面的代码仅仅能处理`IOError`，其他异常需要使用更多的`except`块

一个try语句可以有一个或多个`except`，同时，也可以有一个可选的 `else`和 `finally`语句

```python
try:
    <body>
except <ExceptionType1>:
    <handler1>
except <ExceptionTypeN>:
    <handlerN>
except:
    <handlerExcept>
else:
    <process_else>
finally:
    <process_finally>
```

`except` 语句块像 `elif`；当异常发生时，将检查符合该异常类型的异常块。如果找到，将执行该异常块；最后一个 `exception`中，异常类型将被忽略，不用填写。如果异常发生了，但是不匹配前面的任何类型，那么将执行该最后一个`except`语句块；如果没有异常，那么将执行else语句；而`finally`块不管有没有异常都会执行


例子：
```python
try:
    num1, num2 = eval(input("Enter two numbers, separated by a comma : "))
    result = num1 / num2
    print("Result is", result)
 
except ZeroDivisionError:
    print("Division by zero is error !!")
 
except SyntaxError:
    print("Comma is missing. Enter numbers separated by comma like this 1, 2")
 
except:
    print("Wrong input")
 
else:
    print("No exceptions")
 
finally:
    print("This will execute no matter what")
```

## Raising exceptions

如何主动的引发异常呢？使用`raise`关键字

语法：
```python
raise ExceptionClass("Your argument")
```

例子：
```python
def enterage(age):
    if age < 0:
        raise ValueError("Only positive integers are allowed")

    if age % 2 == 0:
        print("age is even")
    else:
        print("age is odd")

try:
    num = int(input("Enter your age: "))
    enterage(num)

except ValueError:
    print("Only positive integers are allowed")
except:
    print("something is wrong")
```
结果：
```python
Enter your age: -12
Only integers are allowed
```

## Using Exception objects

进入异常处理块后，如何获取到异常对象？

语法如下：
```python
try:
    # this code is expected to throw exception
except ExceptionType as ex:
    # code to handle exception
```

## Creating custom exception class

如何创建自定义的异常呢?需要继承`BaseException`，作为它的子类。

从继承体系中可以看到，我们不一定需要继承`BaseException`,可以根据自己的需要继承对应的类，如`RuntimeError`。

```python
class NegativeAgeException(RuntimeError):
    def __init__(self, age):
        super().__init__()
        self.age = age
```

## Using custom exception class

如何使用自定义异常？和内置的异常是一样的
```python
def enterage(age):
    if age < 0:
        raise NegativeAgeException("Only positive integers are allowed")
 
    if age % 2 == 0:
        print("age is even")
    else:
        print("age is odd")
 
try:
    num = int(input("Enter your age: "))
    enterage(num)
except NegativeAgeException:
    print("Only positive integers are allowed")
except:
    print("something is wrong")
```