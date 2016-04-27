# Python Object and Classes

## Creating object and classes

Python 是面向对象语言，everything都是对象，如 `int` , `str` , `bool`，每一个模块，函数也是对象

### Defining class

类名跟在 `class` 后，以`:`接受；每一个类都有一个特殊的方法，叫做`initializer`，被称作构造器，将在一个对象在创建时被自动自行

例子中，我们定义了一个叫Person的类，包含一个成员`name`和方法 `whoami()`：

```python
class Person:
 
       # constructor or initializer
      def __init__(self, name): 
            self.name = name # name is data field also commonly known as instance variables
 
      # method which returns a string
     def whoami(self):
           return "You are " + self.name
```

### What is self ??

Python中的所有方法中包含一些特殊的方法，如 initializer 有第一个参数self。这个参数用来引用对象以执行方法。当你创建了一个对象，self参数将在`__init__`方法中被自动设置为 **这个类的引用**

## Creating object from class

```python
p1 = Person('tom') # now we have created a new person object p1
print(p1.whoami())
print(p1.name)
# You are tom
# tom
```

当调用一个方法时，不需要通过 `self`参数，Python会自动的完成这个操作。

也能改变数据域
```python
p1.name = 'jerry'
print(p1.name)
```

## Hiding data fields

有时我们不希望一些变量被类的外部引用，即private变量；Python中可以通过在变量名前加入两个下划线作为私有变量，在方法前加入两个下划线作为私有方法

```python
class BankAccount:
 
     # constructor or initializer
    def __init__(self, name, money):
         self.__name = name
         self.__balance = money   # __balance is private now, so it is only accessible inside the class
 
    def deposit(self, money):
         self.__balance += money
 
    def withdraw(self, money):
         if self.__balance > money :
             self.__balance -= money
             return money
         else:
             return "Insufficient funds"
 
    # method which returns a string
    def checkbalance(self):
         return self.__balance
 
b1 = BankAccount('tim', 400)
print(b1.withdraw(500))
b1.deposit(500)
print(b1.checkbalance())
print(b1.withdraw(800))
print(b1.checkbalance())
# Insufficient funds
# 900
# 800
# 100
```

如果在类外部试图访问 `__balance`变量，将抛出错误
```python
AttributeError: 'BankAccount' object has no attribute '__balance'
```
