# Python inheritance and polymorphism

Python 继承多态

当类X继承类Y，那么Y称为超类(super class)或基类(base class),X称为子类(subclass)或派生类(derived class)；记住，只有非private的变量和方法才能被子类访问，private变量和方法只允许在本类内使用

创建子类的语法：
```python
class SubClass(SuperClass):
  # data fields
  # instance methods
```

例子：
```python
class Vehicle:
 
    def __init__(self, name, color):
        self.__name = name      # __name is private to Vehicle class
        self.__color = color
 
    def getColor(self):         # getColor() function is accessible to class Car
        return self.__color
 
    def setColor(self, color):  # setColor is accessible outside the class
        self.__color = color
 
    def getName(self):          # getName() is accessible outside the class
        return self.__name
 
class Car(Vehicle):
 
    def __init__(self, name, color, model):
        # call parent constructor to set name and color  
        super().__init__(name, color)       
        self.__model = model
 
    def getDescription(self):
        return self.getName() + self.__model + " in " + self.getColor() + " color"
 
# in method getDescrition we are able to call getName(), getColor() because they are 
# accessible to child class through inheritance
 
c = Car("Ford Mustang", "red", "GT350")
print(c.getDescription())
print(c.getName()) # car has no method getName() but it is accessible through class Vehicle
```

输出：
```python
Ford MustangGT350 in red color
Ford Mustang
```
`Vehicle`是基类，`Car`是子类；

要调用基类的方法，需要使用`super`关键字；比如要调用基类的`get_information()`的，子类需要使用`super().get_information()`；同样的如果需要调用父类的构造器，子类需要使用`super().__init__()`


## Multiple inheritance

不像C#和Java，Python 允许多继承

语法：
```python
class Subclass(SuperClass1, SuperClass2, ...):
   # initializer
   # methods
```

例子：
```python
class MySuperClass1():
 
    def method_super1(self):
        print("method_super1 method called")
 
class MySuperClass2():
 
    def method_super2(self):
        print("method_super2 method called")
 
class ChildClass(MySuperClass1, MySuperClass2):
 
    def child_method(self):
        print("child method")
 
c = ChildClass()
c.method_super1()
c.method_super2()
```

## Overriding methods

要覆盖基类的方法，子类可以定义同样的签名(signature)的方法实现；签名相同，意味着方法名相同和相同的参数个数

```python
class A():
 
    def __init__(self):
        self.__x = 1
 
    def m1(self):
        print("m1 from A")
 
 
class B(A):
 
    def __init__(self):
        self.__y = 1
 
    def m1(self):
        print("m1 from B")
 
c = B()
c.m1()
# 输出：
# m1 from B
```

## isinstance() function

`isinstance()`函数用于决定一个对象是否是另一个类的实例

语法：
```python
isinstance(object, class_type)
```

例子：
```python
>>> isinstance(1, int)
True
 
>>> isinstance(1.2, int)
False
 
>>> isinstance([1,2,3,4], list)
True
```