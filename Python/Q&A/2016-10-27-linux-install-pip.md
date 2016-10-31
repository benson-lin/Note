# 为Linux 系统安装pip
  
```linux
yum -y install python-devel python-setuptool
wget http://pypi.python.org/packages/source/p/pip/pip-1.0.2.tar.gz
tar zxf pip-1.0.2.tar.gz
cd pip-1.0.2
python setup.py install
```