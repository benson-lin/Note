# Python Modules

Python 模块是一个能存储函数，变量，类，常量等的python文件，模块帮助我们组织相关的代码

## Creating module

```python
# mymodule.py
foo = 100
 
def hello():
    print("i am from mymodule.py")
```

通过`import mymodule`引入

```python
import mymodule
 
print(mymodule.foo)
print(mymodule.hello())
```

## Using from  with import

使用import语句将引入模块中的所有东西，但如果只是需要访问某些内容，可以使用 `from`

```python
# this statement import only foo variable from mymodule
from mymodule import foo  
print(foo)
```

## dir() method

`dir()`是一个内置的方法，用于查找对象的所有的属性(包括所有的类，函数，变量和常量);返回值为列表

语法：
```python
dir(module_name)
```


```python
>>> dir(mymodule)
['__builtins__', '__cached__', '__doc__', '__file__', 
'__loader__', '__name__', '__package__', '__spec__', 'foo', 'hello']

```