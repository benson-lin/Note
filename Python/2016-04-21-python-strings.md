# Python Strings

Python 中的字符串是用 以单引号或双引号包住的相邻一系列字符 表示；Python 没有为单个字符提供数据类型，因此它们总被表示成一个单一的字符串，即使只有一个字符

## Creating strings

```python
>>> name = "tom" # a string
>>> mychar = 'a' # a character
```
或者使用下面的方式

```python
>>> name1 = str() # this will create empty string object
>>> name2 = str("newstring") # string object containing 'newstring'
```

## Strings in python are immutable.

Python 中的字符串是不可变的，意味着一旦字符串被创建，将不能被修改

```python
>>> str1 = "welcome"
>>> str2 = "welcome"
```

`str1` 和 `str2` 引用同样的字符串对象 `"welcome"`，这个对象保存在内存中的某个位置，可以通过`id()`函数查看储存在哪。可以看到引用指向同样的对象

```python
>>> id(str1)
140019671520232
>>> id(str2)
140019671520232
```

如果修改 `str1` 对象的值(确切的说应该是`str1`引用的对象的值，下同)，可以看到 `id(str1)`指向了另一个内存的位置，证明了不能修改原字符串对象，而是创建了新的字符串对象；同样的，Number（`int` 类型）也是不可变的

```python
>>> str1 += " mike"
>>> str1
'welcome mike'
>>> id(str1)
140019671516976
```

## Operations on string

String 下标从 0 开始，`+` 运算符用于连接字符串，'*' 运算符用于复制字符串；下标为 -1 表示最后一个下标

```python
>>> str1[0]
'w'

>>> s = "tom and" + " jerry"
>>> s
'tom and jerry'

>>> s[-1]
'y'

>>> s = "Hello " * 3
>>> s
'Hello Hello Hello '
```

## Slicing string

分割字符串语法：`s[start:end]`。

包含左边界，不包含右边界，即 `start<= x < end` 

```python
>>> str = "Hello"
>>> str[0:2]
'He'
>>> str[2:]
'llo'
>>> str[:4]
'Hell'
```

## ord() and chr() Functions

ord() 函数返回字符的 ASCII 码
chr() 函数返回 ASCII 码对应的字符

```python
>>> ch="b"
>>> ord(ch)
98
>>> chr(98)
```

## String Functions in Python

字符串函数: `len()`, `max()`, `min()`

```python
FUNCTION NAME	FUNCTION DESCRIPTION
len()	        returns length of the string
max()	        returns character having highest ASCII value
min()	        returns character having lowest ASCII value
```

```python
>>> len("hello")
5
>>> max("abc")
'c'
>>> min("abc")
'a'
```

## in and not in  operators

`in`和`not in`操作符用于判断某个字符串是否在另一个字符串中

```python
>>> s = "computer networks"
>>> "comp" in s
True
>>> "err" in s
False
```


## String comparison

可以使用 `>` , `<` , `<=` , `<=` , `==` , `!=`进行字符串的比较；Python通过比较字符的字典顺序实现，如字符的ASCII码

具体看例子：
```python
>>> "tim" == "tie"
False
>>> "free" != "freedom"
True
>>> "arrow" > "aron"
True
>>> "green" >= "glow"
True
>>> "green" < "glow"
False
>>> "ab" < "abc"
True

```


## Iterating string using for loop

使用循环遍历字符串

```python
>>> s="hello"
>>> for ch in s:
...      print(ch)
...
h
e
l
l
o
```

## Testing strings

测试字符串是否属于某种类型，使用下面的函数

```python
METHOD NAME	     METHOD DESCRIPTION
isalnum()	     是否包含字母
isalpha()	     是否只有字母
isdigit()        是否只有数字
isidentifier()	 是否是合法的标识符
islower()	     是否是小写
isupper()	     是否是大写
isspace()	     是否只包含空格
```

```python
>>> s="hello"
>>> s.isalnum()
True
>>> s.isalpha()
True
>>> s
'hello'
>>> s="hell2"
>>> s.isalpha()
False
>>> s.isalnum()
True
>>> s ="_123"
>>> s.isalnum()
False
```

## Searching for Substrings



```python
METHOD NAME	              METHODS DESCRIPTION:
endswith(s1: str): bool	  是否以某字符串结尾
startswith(s1: str): bool 是否以某字符串开头
count(substring): int	  返回某字符串出现的次数
find(s1): int             返回第一次找到的下标，没有找到返回-1
rfind(s1): int	          返回最后一次找到的下标，没有找到返回-1
```

```python
>>> s = "welcome to python"
>>> s.endswith("thon")
True
>>> s.startswith("good")
False
# 第一次出现的下标
>>> s.find("o") 
4
# 最后一次出现的下标
>>> s.rfind("o")
15
# 找不到返回-1
>>> s.find("become")
-1
# 统计字符串出现的次数
>>> s.count("o")
3
```
## Converting Strings

转换字符串函数

```python
METHOD NAME	        METHOD DESCRIPTION
capitalize(): str	首字母大写
lower(): str	    转小写
upper(): str	    转大写
title(): str	    返回每个单词的首字母大写的字符串
swapcase(): str	    大写转小写，小写转大写
replace(old, new): str	替换字符串
```