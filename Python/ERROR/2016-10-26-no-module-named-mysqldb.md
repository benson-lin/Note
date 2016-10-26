**需要安装 MySQL-python**

```python
DATABASES = {
    'default': {
#         'ENGINE': 'django.db.backends.sqlite3',
#         'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        'ENGINE': 'django.db.backends.mysql',
        'NAME':"myblog",
        'USER':'root',
        'PASSWORD':'root',
        'HOST':'127.0.0.1',
        'PORT':'3306'
    }
}
```
django配置数据库启动后发现没安装模块mysqldb


**For Windows**


注意，每次安装成功时都需要使用**重新打开新的cmd**

`https://pypi.python.org/pypi/MySQL-python/1.2.4`里面有github地址：`https://github.com/farcepest/MySQLdb1`，下载后`python setup.py install`安装（或者直接使用`pip install mysql-python`安装）

可能出现`error: Microsoft Visual C++ 9.0 is required (Unable to find vcvarsall.bat)`

因为windows下使用pip安装包的时候需要机器装有vs2008，VS2012还不行，如果不想装VS2008的话，可以安装一个Micorsoft Visual C++ Compiler for Python 2.7的包
(python27在运行setup.py安装时， 会默认寻找visual studio 2008来编译其中的C++文件。 但我安装的是VS2013， 所以需要运行)

再次安装，可能报错：
 `_mysql.c(42) : fatal error C1083: Cannot open include file: 'config-win.h':no such file or directory`

查看` http://dev.mysql.com/downloads/connector/c/6.0.html#downloads`根据python的版本下载32位或64位版本. 默认安装即可. 

如果还出现相同的错误, 请检查文件site.cfg 中connector 的值是否是你安装mysql connector的目录. 默认应该是 `C:\Program Files(x86) \MySQL\MySQL Connector C 6.0.2 (32位)`  或 `C:\Program Files\MySQL\MySQL Connector C 6.0.2 (64位)`

看该文件有没有(x86) ，如果有则下载32位的，没有则下载64位的

最后运行安装`python setup.py install`，提示`Finished processing dependencies for MySQL-python==1.2.4`则成功


there is no MySQLdb for python3.x: Python3不支持MySQLdb


You need to use one of the following commands. Which one depends on what OS and software you have and use.

```text
easy_install mysql-python (mix os)
pip install mysql-python (mix os)
apt-get install python-mysqldb (Linux Ubuntu, ...)
cd /usr/ports/databases/py-MySQLdb && make install clean (FreeBSD)
yum install MySQL-python (Linux Fedora, CentOS ...)
For Windows, see this answer: Install mysql-python (Windows)
```