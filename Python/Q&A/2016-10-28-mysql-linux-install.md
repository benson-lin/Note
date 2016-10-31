下载
http://mirrors.sohu.com/mysql/MySQL-5.6/

找到 mysql-5.6.23-linux-glibc2.5-i686.tar.gz，具体根据操作系统选择

安装
1.添加mysql组和mysql用户
    groupadd mysql
    useradd mysql -g mysql -p mysql
2.解压文件
    tar -zxvf mysql-5.6.23-linux-glibc2.5-i686.tar.gz -C /usr/local/
    重命名
    mv mysql-5.6.23-linux-glibc2.5-i686 mysql
3.进入mysql目录
    cd /usr/local/mysql
4.安装
    scripts/mysql_install_db --user=mysql
5.修改mysql目录权限，否则可能登陆不了
    chgrp -R mysql /usr/local/mysql/
    chown -R mysql /usr/local/mysql/
6.设置服务
    cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
    可以添加到chkconfig中
    chkconfig --add mysqld
    chkconfig --list查看，需要3,4,5状态都为on
    如果有off的:chkconfig --level 345 mysqld on
7.启动mysql
    service mysqld start/stop/restart/status
8.登陆
    /usr/local/mysql/bin/mysql -u root
    默认root是没有密码的

8.设置mysql -u name -p password登陆
  cp /usr/local/mysql/bin/mysql /etc/bin
    或者alias mysql='/usr/loca/mysql/bin/mysql' 加入mysql用户的.bashrc中
9.

第8步
    登陆时可能出现
    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
    因为一些mysql的mysql.sock放在了 /var/lib/mysql/mysql.sock
    可以建立软连接:ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock
    mysql.sock是在mysql启动后才会有的


问题1：cetnos 在运行 scripts/mysql_install_db --user=mysql报错please install the following Perl module

使用yum install -y perl-Module-Install.noarch
然后继续安装就行了



## uninstall

1、使用rpm -qa | grep mariadb搜索 MariaDB 现有的包：
如果存在，使用rpm -e --nodeps mariadb-*全部删除：
2、使用rpm -qa | grep mariadb搜索 MariaDB 现有的包：
如果存在，使用yum remove mysql mysql-server mysql-libs compat-mysql51全部删除；

1、yum remove mysql mysql-server mysql-libs compat-mysql51
2、rm -rf /var/lib/mysql
3、rm /etc/my.cnf

删除mysql服务
[root@localhost local]# chkconfig --list | grep -i mysql
[root@localhost local]# chkconfig --del mysql

删除分散mysql文件夹
[root@localhost local]# whereis mysql 或者 find / -name mysql
mysql: /usr/lib/mysql /usr/share/mysql
清空相关mysql的所有目录以及文件
rm -rf /usr/lib/mysql
rm -rf /usr/share/mysql
rm -rf /usr/my.cnf