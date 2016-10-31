# Apache报ServerRoot must be a valid directory

原因:httpd.conf里面配置的ServerRoot路径跟实际路径不一致，导致路径无效。

打开Apache2.4.16解压文件下的bin文件里面的httpd.conf
本文为：D:\Apache24\conf\httpd.conf

打开httpd.conf后，搜索Define SRVROOT（只有一处），将其后面的双引号里面的路径改为Apache的实际解压路径后保存即可，这里为D:\Apache24

双击重新启动就成功了