# Python Regular Expression


正则表达式被广泛的应用到模式匹配上(pattern matching).Python有对正则函数的内置支持，只需要导入 `re`模块


## re.search() Method

`re.search()`方法用来查找字符串的第一次匹配

语法：`re.search(pattern, string, flags[optional])`

`re.search()` 成功时返回一个 `match` 对象，否则返回 `None`。`match` 对象有一个 `group`函数，用来包含字符串中匹配的文本

flags参数在最后说明

注意：在指定模式时，需要使用纯字符串形式(raw strings)，以`r`作为前缀,如
```python
r'this \n'.
```

在raw strings中,所有的特殊字符和转义字符失去了他们原有的意思，`\n`不再是一行换行字符，现在仅仅只是一个反斜杠，后面跟上一个n

```python
>>> import re
>>> s = "my number is 123"
>>> match = re.search(r'\d\d\d', s)
>>> match
<_sre.SRE_Match object; span=(13, 16), match='123'>
>>> match.group()
'123'
```

正则表达式中`\d`表示需要匹配一个数字，因此`\d\d\d`表示如111,222,784等。



## Basic patterns used in regular expression

```python
SYMBOL	DESCRIPTION
.	    dot matches any character except newline
\w	    matches any word character i.e letters, alphanumeric, digits and underscore ( _ )
\W	    matches non word characters
\d	    matches a single digit
\D	    matches a single character that is not a digit
\s	    matches any white-spaces character like \n, \t, spaces
\S	    matches single non white space character
[abc]	matches single character in the set i.e either match a, b or c
[^abc]	match a single character other than a, b and c
[a-z]	match a single character in the range a to z.
[a-zA-Z]	match a single character in the range a-z or A-Z
[0-9]	match a single character in the range 0-9
^	    match start at beginning of the string
$	    match start at end of the string
+	    matches one or more of the preceding character (greedy match).
*	    matches zero or more of the preceding character (greedy match). 

```

例子：
```python
import re
s = "tim email is tim@somehost.com"
match = re.search(r'[\w.-]+@[\w.-]+', s)

# the above regular expression will match a email address

if match:
    print(match.group())
else:
    print("match not found")
# 输出：tim@somehost.com
```


## Group capturing

组捕获允许从匹配的字符串中抽取部分。你能够使用括号。

假设我们想从上面的例子中的邮件抽取用户名，主机名，我们使用下面的语句

```python
match = re.search(r'([\w.-]+)@([\w.-]+)', s)
```

注意：括号不能改变字符串的匹配(可以看成加上括号的效果和去掉括号的效果一样)，如果匹配成功，那么`match.group(1)`获取第一个括号的内容，`match.group(2)`获取第二个括号的内容

```python
import re
s = "tim email is tim@somehost.com"
match = re.search('([\w.-]+)@([\w.-]+)', s)
if match:
    print(match.group()) ## tim@somehost.com (the whole match)
    print(match.group(1)) ## tim (the username, group 1)
    print(match.group(2)) ## somehost (the host, group 2)
```


## findall() Function

前面说过，`re.search()`只找到第一个符合模式的字符串，如果我们想获取全部匹配的字符串，可以使用`findall()`函数。返回值为一个 List，如果没有找到匹配，则返回空的List

```python
import re
s = "Tim's phone numbers are 12345-41521 and 78963-85214"
match = re.findall(r'\d{5}', s)
 
if match:
    print(match)
    
# 输出：['12345', '41521', '78963', '85214']
```

也可以在`findall()`中使用组捕获。如果组捕获被使用，那么`findall`将返回包含多个元组(tuples)的List，下面的例子清楚的解释了一切

```python
import re
s = "Tim's phone numbers are 12345-41521 and 78963-85214"
match = re.findall(r'(\d{5})-(\d{5})', s)
print(match)
 
for i in match:
    print()
    print(i)
    print("First group", i[0])
    print("Second group", i[1])
```

输出：
```python
[('12345', '41521'), ('78963', '85214')]
 
('12345', '41521')
First group 12345
Second group 41521
 
('78963', '85214')
First group 78963
Second group 85214
```

## Optional flags


`re.search()`和`re.findall()`都可接受可选的叫做flags的参数，这个参数用来修改模式匹配行为


```python
FLAGS	        DESCRIPTION
re.IGNORECASE	忽略大写和小写
re.DOTALL	    Allows (.) to match newline, be default (.) matches any character except newline
re.MULTILINE	This will allow ^ and $ to match start and end of each line
```

## Using re.match()

`re.match()`与`re.search()`相似，不同点在于它将在字符串的起始端开始寻找匹配。

```python
import re
s = "python tuts"
match = re.match(r'py', s)
if match:
    print(match.group())
```

能够通过在`re.search()`函数中添加`^`达到相同的效果

```python
import re
s = "python tuts"
match = re.search(r'^py', s)
if match:
    print(match.group())
```

