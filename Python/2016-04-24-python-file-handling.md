# Python File Handling

## Opening a file

在读或写文件时，需要先打开文件，语法如下：
```python
f = open(filename, mode)
```
`open()` 函数接受两个字符串参数，`filename`指明文件的路径，`mode`则用来标识文件是用来读还是写，f是文件处理对象，也被叫做文件指针(file pointer)

## Closing a file

当完成读或写文件后，需要使用`close()`函数关闭
```python
f.close()  # where f is a file pointer
```

## Different modes of opening a file are

打开一个文件的不同模式，如下：
```python
MODES	   DESCRIPTION
"r" 	   打开一个只读文件
"w" 	   如果文件存在将在打开文件前清除数据，否则创建新的文件
"a" 	   打开append模式，将数据写入文件的末尾
"wb" 	   以二进制写模式打开一个文件
"rb" 	   以二进制读模式打开一个文件
```


## Writing data to the file

```python
>>> f = open("D:/test.txt","w")
>>> f.write("the first line\n")
15
>>> f.write("the second line\n")
16
>>> f.close()
```

## Reading data from the file

读取文件有如下三个方法
```python
METHODS  	    DESCRIPTION
read([number]) 	返回特定数量的字母，如果忽略将读取文件的全部内容
readline() 	    返回一行
readlines() 	返回所有行，放到list中
```

```python
## 读取所有内容
>>> f = open("D:/test.txt","r")
>>> f.read()
'the first line\nthe second line\n'
>>> f.close()
# 读取前5个字符
>>> f = open("D:/test.txt","r")
>>> f.read(5)
'the f'
>>> f.close()
# 一次读取一行
>>> f = open("D:/test.txt","r")
>>> f.readline()
'the first line\n'
>>> f.readline()
'the second line\n'
>>> f.readline()
''
>>> f.close()
# 一次读取一行，读取全部行放到列表中
>>> f = open("D:/test.txt","r")
>>> f.readlines()
['the first line\n', 'the second line\n']
>>> f.close()
```

## Appending data

追加数据，使用 `a`模式

```python
>>> f = open("D:/test.txt","a")
>>> f.write("the third line\n")
15
>>> f.close()
>>> f = open("D:/test.txt","r")
>>> f.read()
'the first line\nthe second line\nthe third line\n'
```

## Binary reading and writing

为了使用二进制的i/o，需要使用 `pickle`模块，这个模块使用`load`函数作为读，`dump`函数作为写

### Writing data

```python
>> import pickle
>>> f = open('pick.dat', 'wb')
>>> pickle.dump(11, f)
>>> pickle.dump("this is a line", f)
>>> pickle.dump([1, 2, 3, 4], f)
>>> f.close()
```

### Reading data

```python
>> import pickle
>>> f = open('pick.dat', 'rb')
>>> pickle.load(f)
11
>>> pickle.load(f)
"this is a line"
>>> pickle.load(f)
[1,2,3,4]
>>> f.close()
```

如果没有更多数据可读，将抛出EOFError错误