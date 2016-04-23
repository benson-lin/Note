# Python Control Statements

Python 中的控制语句使用 `if..else...`

语法：
```python
if boolean-expression:
   #statements
else:
   #statements
```
注意：在if块的每个语句必须使用相同数量的空格缩进，否则会提示语法错误，对其它语言如Java,C#不同，这些语言使用大括号{}表示语句块

然后需要知道关系运算符(Relational operators)，关系运算符用于比较两个对象，结果返回boolean值，即True或False


```python
SYMBOL	DESCRIPTION
<=   	smaller than or equal to
< 	    smaller than
> 	    greater than
>= 	    greater than or equal to
== 	    equal to
!= 	    not equal to
```


例子：
```python
i = 10
 
if i % 2 == 0:
   print("Number is even")
else:
   print("Number is odd")
```


如果条件检查有一长串，要使用`if-elif-else`语句

语法：
```python
if boolean-expression:
   #statements
elif boolean-expression:
   #statements
elif boolean-expression:
   #statements
else:
   #statements
```






