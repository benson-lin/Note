# c++ using与#include 困惑

最近在学习c++，举个例子，程序前面已经#include <string> ,为什么后面使用string str；定义变量的时候，还要在前面加上 `using std::string` ，难道`#include<string>` 的时候，没有把string的 定义引用过来吗？ 还需要using 二次声明一下？


```c++
#include "stdafx.h"
#include <iostream>
#include <string>

using std::cin;
using std::cout;
using std::string;
using std::endl;

int main()
{
	string s1;
	string s2("test");
	string s3(s2);
	string s4(5, '4');
	

	string word;
	while (cin >> word)
	{
		cout << word << endl;
	}
	return 0;

}
```


\#include <string>把string的定义引用过来了，但是别的库文件可能也会定义自己的string类型（你也可以尝试这么做）。

为了防止跟其他string类型冲突，C++标准库把string定义到std名称空间里面，所以要用using std::string 指定用到的是std名称空间里面的string类型，而不是别的库文件里面的string

using namespace std是不推荐的，因为标准库定义了数量庞大的名称，包括min、max、ref、swap等常用名称，一不小心发生重名的话就会有难以预料的BUG发生