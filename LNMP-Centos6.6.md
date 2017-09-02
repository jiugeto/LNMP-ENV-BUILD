Linux-Nginx-Mysql-Php 环境搭建

# LNMP-Centos6.6，手动编译安装开发环境

```
sql
cd /etc/sysconfig/network-scripts
cp ifcfg-eth0 ./ifcfg-eth0.bak
vi ifcfg-eth0
ONBOOT=yes  #开机自动开启
vi /etc/selinux/config
SELINUX=disabled  #关闭selinux
```

配置网络yum源
```
sql
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bk
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
yum makecache
yum update -y yum
yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers
```

nginx安装
```
sql
useradd -s /sbin/nologin -M nginx
groupadd ngnix
cd /home/jiuge/tar
tar -zxvf nginx-1.3.1.tar.gz
./configure --user=nginx --group=nginx --prefix=/usr/local/nginx-1.3.1 --with-http_stub_status_module --with-http_ssl_module --with-pcre=/home/jiuge/tar/pcre-8.33
make && make install
ln -s /usr/local/nginx-1.3.1 /usr/local/nginx
/usr/local/nginx/sbin/nginx -t
netstat -anpt | grep nginx
ps -ef | grep nginx
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
/usr/local/nginx/sbin/nginx -s reload
yum install gcc pcre-devel zlib-devel openssl
cd /home/jiuge/tar
tar -zxvf nginx-1.8.0.tar.gz
./configure --prefix=/usr/local/nginx
make && make install
vi /etc/init.d/nginx
	-- 输入下面的代码
	#!/bin/bash
	# nginx Startup script for the Nginx HTTP Server
	# it is v.0.0.2 version.
	# chkconfig: - 85 15
	# description: Nginx is a high-performance web and proxy server.
	#              It has a lot of features, but it's not for everyone.
	# processname: nginx
	# pidfile: /var/run/nginx.pid
	# config: /usr/local/nginx/conf/nginx.conf
	nginxd=/usr/local/nginx/sbin/nginx
	nginx_config=/usr/local/nginx/conf/nginx.conf
	nginx_pid=/var/run/nginx.pid
	RETVAL=0
	prog="nginx"
	# Source function library.
	. /etc/rc.d/init.d/functions
	# Source networking configuration.
	. /etc/sysconfig/network
	# Check that networking is up.
	[ ${NETWORKING} = "no" ] && exit 0
	[ -x $nginxd ] || exit 0
	# Start nginx daemons functions.
	start() {
	if [ -e $nginx_pid ];then
	   echo "nginx already running...."
	   exit 1
	fi
	   echo -n $"Starting $prog: "
	   daemon $nginxd -c ${nginx_config}
	   RETVAL=$?
	   echo
	   [ $RETVAL = 0 ] && touch /var/lock/subsys/nginx
	   return $RETVAL
	}
	# Stop nginx daemons functions.
	stop() {
	        echo -n $"Stopping $prog: "
	        killproc $nginxd
	        RETVAL=$?
	        echo
	        [ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx /var/run/nginx.pid
	}
	# reload nginx service functions.
	reload() {
	    echo -n $"Reloading $prog: "
	    #kill -HUP `cat ${nginx_pid}`
	    killproc $nginxd -HUP
	    RETVAL=$?
	    echo
	}
	# See how we were called.
	case "$1" in
	start)
	        start
	        ;;
	stop)
	        stop
	        ;;
	reload)
	        reload
	        ;;
	restart)
	        stop
	        start
	        ;;
	status)
	        status $prog
	        RETVAL=$?
	        ;;
	*)
	        echo $"Usage: $prog {start|stop|restart|reload|status|help}"
	        exit 1
	esac
	exit $RETVAL
chmod a+x /etc/init.d/nginx
vi /etc/rc.d/rc.local
/etc/init.d/nginx start
```

mysql安装
```
sql
yum install cmake
cd /home/jiuge/tar
tar zxvf mysql-5.6.27.tar.gz
cd mysql-5.6.27
cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DSYSCONFDIR=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DMYSQL_TCP_PORT=3306  \
-DENABLE_DOWNLOADS=1
make && make install
cp support-files/my-medium.cnf /etc/my.cnf
useradd mysql
chmod +x /usr/local/mysql
	# 更改文件拥有者
chown -R mysql.mysql /usr/local/mysql
	# 初始化MySQL数据库
/usr/local/mysql/scripts/mysql_install_db \
--user=mysql \
--basedir=/usr/local/mysql \
--datadir=/usr/local/mysql/data &
	# MySQL的安装文件中，data以外的主人改为root，避免数据库恢复出厂设置
chown -R root /usr/local/mysql
chown -R mysql /usr/local/mysql/data
	# 后台运行MySQL服务
/usr/local/mysql/bin/mysqld_safe --user=mysql &
	# 查看MySQL是否启动
ps -A | grep mysql
	# 测试数据库
/usr/local/mysql/bin/mysql -u root
-- 连接mysql失败并报错
ps -A | grep mysql
kill -9 进程号
/usr/local/mysql/bin/mysqld_safe --user=mysql &
/usr/local/mysql/bin/mysql -u root
show databases;
	# cp安装包解压目录
cp /support-files/mysql.server /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld
chkconfig --add mysqld
chkconfig mysqld on
	# 配置文件路径
vi /etc/rc.d/rc.local
	# 文件中增加代码
/usr/local/mysql/bin/mysqld_safe --user=mysql &
service vsftpd start
```

libgd
```
sql
cd /home/jiuge/tar
tar -zxvf libgd-2.1.1.tar.gz
cd libgd-2.1.1
./configure --prefix=/usr/local/gd --with-zlib=/usr/local/zlib --with-png=/usr/local/libpng --with-jpeg=/usr/local/jpeg --with-freetype=/usr/local/freetype --with-fontconfig=/usr/local/fontconfig
make && make install
```

安装libiconv
```
sql
cd /home/jiuge/tar
tar -zxvf libiconv-1.14.tar.gz
cd libiconv-1.14
./configure --prefix=/usr/local/libiconv
make && make install
```

php安装
```
sql
yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers
cd /home/jiuge/tar
tar -zxvf php-5.6.2.tar.gz
cd php-5.6.2
./configure  --prefix=/usr/local/php  --with-pdo-mysql=mysqlnd   --with-mysqli=mysqlnd   --with-mysql=mysqlnd  --with-xmlrpc --with-openssl --with-zlib --with-freetype-dir=/usr/local/freetype --with-gd --with-jpeg-dir=/usr/local/jpeg --with-png-dir=/usr/local/libpng --with-iconv=/usr/local/libiconv --enable-short-tags --enable-sockets --enable-soap --enable-mbstring --enable-static --enable-gd-native-ttf --with-curl --with-xsl --enable-ftp --with-libxml-dir=/usr/local/libxml2 --enable-gd-native-ttf --enable-zip --enable-fpm
-- 报错：xslt-config not found. Please reinstall the libxslt >= 1.1.0 distribution
yum install libxslt-devel
-- 再编译
make && make install
```

php-fpm配置
```
sql
cd /usr/local/php5.6.2/etc/
cp php-fpm.conf.default php-fpm.conf
egrep -v ";|^$" php-fpm.conf
	[global]
	error_log = /var/log/php-fpm.log
	log_level = notice
	pid = /var/log/php/php-fpm.pid
	[www]
	user = nginx
	group = nginx
	listen = 127.0.0.1:9000
	-- pm = dynamic
	-- pm.max_children = 50
	-- pm.start_servers = 20
	-- pm.min_spare_servers = 5
	-- pm.max_spare_servers = 35
mkdir /var/log/php
/usr/local/php/sbin/php-fpm
ps -ef | grep php-fpm
-- 修改php.ini
cp /home/jiuge/tar/php5.6.2/php.ini-development /usr/local/php/php.ini
short_open_tag = On
grep short_open_tag /usr/local/php5.4.6/php.ini
-- php-fpm开机自启
vi /etc/rc.d/rc.local
/usr/local/php/sbin/php-fpm &
```

整合nginx和php
```
sql
vi /usr/local/nginx/conf/nginx.conf
cd /var/log/nginx_error.log  #nginx日志目录
cd /var/log/www_logs/  #站点日志目录
cd /var/www/  #lnmp多站点目录
......
```
