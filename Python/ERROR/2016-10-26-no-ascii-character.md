
`SyntaxError: Non-ASCII character '\xe7' in file  `

出现这种错误的原因是程序中的编码出问题了，只要在程序的最前面加上

最前面的意思是在最前面，包括在注释的前面


```python
#-*- coding: UTF-8 -*-   
```

保存运行即可