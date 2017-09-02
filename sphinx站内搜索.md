# Sphinx站内搜索

sphinx-2.2.11-release.tar.gz、coreseek-3.2.14.tar.gz

```
sql
-- 下载：http://sphinxsearch.com/downloads/sphinx-2.2.11-release.tar.gz
cd /home/jiuge/tar/sphinx-2.2.11-release
./configure \
--prefix=/usr/local/sphinx 
make && make install
-- 安装完成后，/usr/local/sphinx下面有4个目录：bin、etc、share、var
cd /usr/local/sphinx/etc
cp sphinx-min.conf.dist sphinx.conf
-- 配置sphinx：数据源、索引定义、生成索引、searchd服务定义
vi sphinx.conf
-- 生成索引
/usr/local/sphinx/bin/indexer --all
-- 查看Linux系统版本
file /bin/ls
-- 报错：处理
ln -s /usr/local/mysql/lib/libmysqlclient.so.18 /usr/lib		--32位
-- ln -s /usr/local/mysql/lib/libmysqlclient.so.18.0.0 /usr/lib64/libmysqlclient.so.18		--64位


-- 安装coreseek的：mmseg中文分词包，crsf(sphinx包)
cd /home/jiuge/tar/coreseek-3.2.14
-- 安装mmseg
cd mmseg-3.2.14
./configure --prefix=/usr/local/mmseg
-- 报错error: cannot find input file: src/Makefile.in
yum -y install libtool
automake
aclocal
libtoolize -f
automake -a
autoconf
autoheader
make clean
./configure --prefix=/usr/local/mmseg
make && make install
-- 安装csft
cd /home/jiuge/tar/coreseek-3.2.14/csft-3.2.14
sh buildconf.sh
./configure \
--prefix=/usr/local/coreseek \
--with-mysql=/usr/local/mysql \
--with-mmseg=/usr/local/mmseg \
--with-mmseg-includes=/usr/local/mmseg/include/mmseg/ \
--with-mmseg-libs=/usr/local/mmseg/lib/ \
--with-mysql
make && make install

-- 配置csft
cd /usr/local/coreseek/etc
cp sphinx.conf.dist csft.conf
vi csft.conf
-- 在index XXX的括号下面加上：
charset_type = zh_cn.utf-8
charset_dictpath =/usr/local/mmseg/etc/
-- 建立索引
/usr/local/coreseek/bin/indexer --all


-- laravel sphinx整合
-- 安装依赖libsphinxclient
cd /home/jiuge/tar/coreseek-3.2.14/csft-3.2.14/api/libsphinxclient/
./configure  --prefix=/usr/local/sphinx 
make && make install
-- 安装sphinx的php扩展
wget http://pecl.php.net/get/sphinx-1.3.0.tgz
tar -zxvf sphinx-1.3.0.tgz
cd /home/jiuge/tar/sphinx-1.3.0
phpize
./configure \
--with-php-config=/usr/local/php/bin/php-config \
--with-sphinx=/usr/local/sphinx 
make && make install 
-- cd /etc/php.d/ 
-- cp gd.ini  sphinx.ini
-- 没有gd.ini？
-- yum install php-gd 
-- vi sphinx.ini 
-- extension=sphinx.so
-- 重启php-fpm
-- 修改php.ini，
-- 将php.ini拷到phpinfo()的Configuration File (php.ini) Path目录中：/usr/local/php/lib
vi /usr/local/php/php.ini
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/"
extension=sphinx.so
find / -name php
kill XXX
/usr/local/php/sbin/php-fpm &
/etc/init.d/nginx restart
-- 查看php扩展
php -m

-- 启动sphinx
-- /usr/local/sphinx/bin/searchd
-- 启动coreseek
/usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft.conf 
-- 关闭
/usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft.conf --stop
-- 建立索引
/usr/local/coreseek/bin/indexer -c /usr/local/coreseek/etc/csft.conf --all --rotate  
-- 增量索引
/usr/local/coreseek/bin/indexer -c /usr/local/coreseek/etc/csft.conf delta --rotate
-- 合并索引
/usr/local/coreseek/bin/indexer -c /usr/local/coreseek/etc/csft.conf --merge main delta --rotate --merge-dst-range deleted 0 0  
```
