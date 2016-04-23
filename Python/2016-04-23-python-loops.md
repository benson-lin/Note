# Python Loops

Python 只有两种循环方式：`for`循环和`while`循环

## For loop

语法：
```python
for i in iterable_object:
   # do something
```

## range(a, b) Function

`range(a, b)`函数用于返回a,a+1,,,,b-2,b-1的整数序列；自行查看`help(range)`

```python
for i in range(1, 10):
    print(i)
# 打印1-9
```

这个函数还有个可选参数，作为步长

```python
for i in range(1, 20, 2):
    print(i)
# 输出1 3 5 7...
```

## While loop

语法：
```python
while condition:
    # do something
```


## break statement & continue statement

可以配合`break`和`continue`语句
```python
count = 0
 
while count < 10:
    count += 1
    if count == 5:
         break    
    print("inside loop", count)
```
```python
count = 0
 
while count < 10:
    count += 1
    if count % 2 == 0:
           continue
    print(count)
```
