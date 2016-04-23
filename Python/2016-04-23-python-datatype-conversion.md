# Datatype conversion

如何进行数据类型转换

## Converting int to float

```python
>>> i = 10
>>> float(i)
10.0
```


## Converting float to int

```python
>>> f = 14.66
>>> int(f)
14
```

## Converting string to int
如何包含非数字的字符会抛出 ValueError 
```python
>>> s = "123"
>>> int(s)
123
```

## Converting number to string

```python
>>> i = 100
>>> str(i)
"100"
>>> f = 1.3
str(f)
'1.3'
```


## Rounding numbers

四舍五入
```python
>>> i=23.5667
>>> round(i,2)
23.57
```