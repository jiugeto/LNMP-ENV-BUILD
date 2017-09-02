# LNMP-ENV-BUILD
Linux-Nginx-Mysql-Php 环境搭建

## LNMP-Centos6.6

```
sql
cd /etc/sysconfig/network-scripts
cp ifcfg-eth0 ./ifcfg-eth0.bak
vi ifcfg-eth0
ONBOOT=yes  #开机自动开启
vi /etc/selinux/config
SELINUX=disabled  #关闭selinux
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bk
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
yum makecache
```
