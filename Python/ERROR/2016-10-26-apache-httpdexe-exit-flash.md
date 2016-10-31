# Apache闪退问题

现象：双击httpd.exe闪退

解决方法：肯定是启动报错导致的，可以通过cmd进入bin目录再次运行httpd，因为是命令行形式的，所以不会闪退，看到错误信息，百度查找问题解决。一般的错误是找不到 SERVERROOT 目录，可以找到httpd.conf，搜索找到Define SRVROOT，将值修改为Apache所在根目录解决，如D:\Apache24