
## 80端口冲突：

make_sock: c ould not bind to address 0.0.0.0:80 
no listening sockets available, shutting down Unable to open logs

找到Apache目录下的 conf/httpd.conf

按Ctrl+f查找内容 80 找到以下两处 

```text
#Listen 12.34.56.78:80 Listen 80  

# If your host doesn't have a registered DNS name, enter its IP address here. 
# You will have to access it by its address anyway, and this will make 
 # redirections work in a sensible way. # 
ServerName localhost:80 
```
修改冲突端口保存即可


## 443端口冲突

到Apache24\conf\extra目录下找到httpd-ssl.conf文件，记事本打开，找到“Listen 443”，修改值

443端口冲突一般是因为安装了VMWARE导致的。可以通过`netstat -o -a`查看是哪个Pid占用了端口，然后在任务管理器中找到这个Pid并将它停止
