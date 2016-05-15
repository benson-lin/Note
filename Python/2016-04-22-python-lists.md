# Python Lists

list 列表类，允许添加删除和访问元素。

## Creating list in python

使用`[]`

```python
>>> l = [1, 2, 3, 4]
>>> l2 = ["this is a string", 12] #也可以是不同类型的
>>> list1 = list() # Create an empty list
>>> list2 = list([22, 31, 61]) # Create a list with elements 22, 31, 61
>>> list3 = list(["tom", "jerry", "spyke"]) # Create a list with strings
>>> list5 = list("python") # Create a list with characters p, y, t, h, o, n
>>> l[0] # 访问元素
```


注意：Lists 是可变的

## List Common Operations

下面 list 的常用操作并不属于 list 类，list 类的操作应当形如`list.function()`

`in`，`not in`，`len()`，`max()`，`min()`，`sum()`

```python
>>> list1 = [2, 3, 4, 1, 32]
>>> 2 in list1
True
>>> 33 not in list1
True
>>> len(list1) # find the number of elements in the list
5
>>> max(list1) # find the largest element in the list
32
>>> min(list1) # find the smallest element in the list
1
>>> sum(list1) # sum of elements in the list。只对数字有效
42
```



## List slicing

List 切割，和 String 的切割相似

```python
>>> list = [11,33,44,66,788,1]
>>> list[0:5] # this will return list starting from index 0 to index 4
[11,33,44,66,788]
>>> list[:3] 
[11,33,44]
# 可以获取倒数的序列
>>> list = [11,33,44,66,788,1]
>>> list[-3:]
[66, 788, 1]
>>> list[-3:-2]
[66]
# 还可以设置步长
>>> list[0:5:2]
[11, 44, 788]

```

## +  and *  operators in list

Python 中，list中有比较有趣的操作符，就是 + 和 * ，分别用来添加元素和复制元素
```python
>>> list1=[11,33]
>>> list2=[1,6]
>>> list3=list1+list2
>>> list3
[11, 33, 1, 6]
>>> list4=list2*3
>>> list4
[1, 6, 1, 6, 1, 6]
```

## Traversing list using for loop

遍历list，没什么说的
```python
>>> list = [1,2,3,4,5]
>>> for i in list:
... print(i, end=" ")
1 2 3 4 5
```

## Commonly used list methods with return type

还有很多操作，记得学会用`help`命令

```python
METHOD                   NAME DESCRIPTION
append(x:object):None	 添加到list尾部，返回None
count(x:object):int	     返回x元素出现的次数
extend(l:list):None	     添加l的所有元素到list中，返回None，相当于+
index(x: object):int 	 返回x元素第一次出现的位置
insert(index: int, x: object):None 	某个位置插入元素
remove(x:object):None 	 删除list中出现的第一个x元素
reverse():None 	         反转list
sort(): None 	         升序排序
pop(i): object 	         返回i位置的元素并删除
pop(): object            返回最后一个元素，并删除
```

```python
>>> list
[11, 33, 6, 1, 6]
>>> list.append(9)
>>> list
[11, 33, 6, 1, 6, 9]
>>> list.count(6)
2
>>> list2=[7,8,9]
>>> list.extend(list2)
>>> list
[11, 33, 6, 1, 6, 9, 7, 8, 9]
>>> list
[11, 33, 6, 1, 6, 9, 7, 8, 9]
>>> list.insert(2,0)
>>> list
[11, 33, 0, 6, 1, 6, 9, 7, 8, 9]
>>> list.remove(6)
>>> list
[11, 33, 0, 1, 6, 9, 7, 8, 9]
>>> list.reverse()
>>> list
[9, 8, 7, 9, 6, 1, 0, 33, 11]
>>> list.sort()
>>> list
[0, 1, 6, 7, 8, 9, 9, 11, 33]
# 如果是直接赋值实际上是引用赋值
>>> list = [11, 33, 0, 1, 6, 9, 7, 8, 9]
>>> y = list
>>> y.sort()
>>> y
[0, 1, 6, 7, 8, 9, 9, 11, 33]
>>> list
[0, 1, 6, 7, 8, 9, 9, 11, 33]
## 正确排序需要下面这样
>>> list = [11, 33, 0, 1, 6, 9, 7, 8, 9]
>>> y = sorted(list)
>>> y
[0, 1, 6, 7, 8, 9, 9, 11, 33]
>>> list
[11, 33, 0, 1, 6, 9, 7, 8, 9]
>>> list.pop()
33
>>> list
[0, 1, 6, 7, 8, 9, 9, 11]
>>> list.pop(2)
6
>>> list
[0, 1, 7, 8, 9, 9, 11]
>>>
```


`list.extend()`和和用`+`合并起来的结果是一样的，但是是有区别的：
第一种情况是被list本身没被替换，而第二种连接操作是创建了一个新的副本，因此`a.extend(b)`比`a=a+b`的效率更高