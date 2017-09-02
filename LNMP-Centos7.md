# LNMP-Centos7，yum安装

```
sql
-- 安装centos7时报错：CPU被禁用
-- 打开 .vmx后缀的操作系统配置文件，加入以下代码
-- cpuid.1.eax = "0000:0000:0000:0001:0000:0110:1010:0101"
-- 关闭虚拟机电源，再次启动虚拟机

-- 要添加的CentOS7 Nginx的yum软件库，打开SSH终端，并使用下面的命令
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
-- 安装
sudo yum install nginx
-- 开启
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
-- 关闭selinux
setenforce 0
vi /etc/selinux/config
SELINUX=disabled
-- 关闭防火墙
systemctl stop firewalld.service  
systemctl disable firewalld.service 
-- 然后重启Linux，才能生效
-- 以下是Nginx的默认路径：
-- (1) Nginx配置路径：/etc/nginx/
-- (2) PID目录：/var/run/nginx.pid
-- (3) 错误日志：/var/log/nginx/error.log
-- (4) 访问日志：/var/log/nginx/access.log
-- (5) 默认站点目录：/usr/share/nginx/html

-- 添加 yum 源
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum install php70w
yum install php70w-fpm
yum install php70w-mysqlnd
yum install php70w-pecl-redis
yum install php70w-mbstring
yum install php70w-gd
-- 启动php-fpm
service php-fpm start

-- 安装redis
yum install redis
service redis start

-- 添加 yum 源
rpm -Uvh http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
yum install mysql-community-server
systemctl start mysqld.service
-- 查看MySQL初始密码
grep 'password' /var/log/mysqld.log
mysql -u root -p
-- 密码重置：大小写字母、数字、特殊字符
SET password for 'root'@'localhost'=password('Zwx639291##');
-- 允许 Navicat 连接数据库
grant all privileges on *.* to 'root'@'%' identified by 'Zwx639291##' with grant option;



-- 服务管理
# 启动 MySQL
systemctl start mysqld.service
# 停止 MySQL
systemctl stop mysqld.service
# 重启 MySQL
systemctl restart mysqld.service
# 启动 PHP
systemctl start php-fpm.service
# 停止 PHP
systemctl stop php-fpm.service
systemctl enable php-fpm.service
# 重启 PHP
systemctl restart php-fpm.service
# 启动 Nginx
systemctl start nginx.service
# 停止 Nginx
systemctl stop nginx.service
# 重启 Nginx
systemctl restart nginx.service


-- composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
cd /www/
composer global require "laravel/installer"
composer create-project --prefer-dist laravel/laravel blog
```
