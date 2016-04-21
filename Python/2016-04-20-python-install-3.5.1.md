# Python3.5.1 Install


使用yum install python得到的是centos认为的稳定的版本，如果需要替换成新的怎么办？

```python
# which python 
/usr/bin/python 
# python --version 
Python 2.6.6
```

根据需要修改版本号
可以网页访问http://www.python.org/ftp/python/查看需要的版本

如果远程(如腾讯云)下载太慢，可以在自己电脑本地下载后上传到远程(翻墙下载应该更快)

```python
wget http://www.python.org/ftp/python/3.5.1/Python-3.5.1.tar.xz

# xz需要使用 xz库解压，系统可能没有，需要安装
yum install xz-libs

## 假设下载到 /tmp 目录下
xz -d /tmp/Python-3.5.1.tar.xz
tar -xvf /tmp/Python-3.5.1.tar

# Enter the file directory:
cd /tmp/Python-3.5.1

# Start the configuration (setting the installation directory)
# By default files are installed in /usr/local.
# You can modify the --prefix to modify it (e.g. for $HOME).
./configure --prefix=/usr/local   


# Let's build (compile) the source
# This procedure can take awhile (~a few minutes)
make

# After building everything:
make altinstall
```

```python
# 替换2.7，查看python命令在哪
[root@VM_221_211_centos bin]# which python
/usr/bin/python
[root@VM_221_211_centos bin]# cd /usr/bin
# 查看python命令,其实是软链接
[root@VM_221_211_centos bin]# ls -ll py*
# 根据列出的删除引用到的2.7.1的版本
[root@VM_221_211_centos bin]# sudo rm python python2 python2.7
## 建立新的软链接，/usr/local/bin/python3.5为新版本的python命令的地址
[root@VM_221_211_centos bin]# ln -s /usr/local/bin/python3.5 python
# 查看版本
[root@VM_221_211_centos bin]# python --version
Python 3.5.1
```