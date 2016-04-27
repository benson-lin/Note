# Python Generators


生成器(Generator) 用于创建迭代器的函数，为了能够在函数外的循环被使用


## Creating Generators

生成器的定义与函数相似，但有一点不同，它记住上一次返回时在函数体中的位置。对生成器函数的第二次调用跳转回函数中间，而上次调用的所有局部变量都保持不变。

我们使用`yield`关键字返回每次循环时的某个值。

下面的例子尝试仿造python中的range()函数；
这个例子中使用了默认值，

```python
def my_range(start, stop, step = 1):
    if stop <= start:
        raise RuntimeError("start must be smaller than stop")
    i = start
    while i < stop:
        yield i # here
        i += step
 
try:
    for k in my_range(10, 50, 3):
        print(k)
except RuntimeError as ex:
    print(ex)
except:
    print("Unknown error occurred")
```


输出:
```python
10
13
16
19
22
25
28
31
34
37
40
43
46
49
```

可以看到在每次循环时，i的值作为返回值返回了，在调用函数处被使用，上面的例子是k值。所有在每一个迭代过程中，i的值都赋给了k