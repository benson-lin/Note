# Linux安装Apache


首先，到apache的官网下载 http://httpd.apache.org/download.cgi

找到类似的这行 `Source: httpd-2.4.23.tar.gz` 下载


解压

chmod 755 httpd-2.4.23.tar.gz   (说明:给予更多的权限)

./configure --prefix=/usr/local/apache --enable-module=most --enable-shared=max

(说明:配置Apache。这里我把默认可以生成的"httpd"改成了"apache"的目录,目的为了便于查找)

make   (说明:编译Apache)

make install  (说明:安装Apache)


## Linux上安装Apache时，编译可能出现错误： 
 
```linux
checking for APR... no  
configure: error: APR not found .  Please read the documentation
```

### apr not found问题

```linux
tar -zxf apr-1.4.5.tar.gz  
cd  apr-1.4.5  
./configure --prefix=/usr/local/apr  
make && make install  
```

### APR-util not found问题

```linux
tar -zxf apr-util-1.3.12.tar.gz  
cd apr-util-1.3.12  
./configure --prefix=/usr/local/apache --enable-so --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util/ --with-pcre=/usr/local/pcre  
make && make install  
```

#### pcre问题

```linux
unzip -o pcre-8.10.zip  
cd pcre-8.10  
./configure --prefix=/usr/local/pcre  
make && make install  
```


# 启动
本文假设你的apahce安装目录为/usr/local/apache，这些方法适合任何情况
apahce启动命令：
推荐/usr/local/apache/bin/apachectl start apaceh启动
apache停止命令
/usr/local/apache/bin/apachectl stop   停止
apache重新启动命令：
/usr/local/apache/bin/apachectl restart 重启
要在重启 Apache 服务器时不中断当前的连接，则应运行：
/usr/local/sbin/apachectl graceful
如果apache安装成为linux的服务的话，可以用以下命令操作：
service httpd start 启动
service httpd restart 重新启动
service httpd stop 停止服务