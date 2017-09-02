# Linux-Samba配置

查看Samba是否已安装
```
sql
rpm -qa | grep samba
```

安装配置
```
sql
yum -y install samba samba-client
vi /etc/samba/smb.conf
[global]
	security=share	
[www]
path=/var/www
public=yes
writable = yes
```

smbpasswd命令
```
sql
smbpasswd -a 增加用户（要增加的用户必须以是系统用户）
smbpasswd -d 冻结用户，就是这个用户不能在登录了
smbpasswd -e 恢复用户，解冻用户，让冻结的用户可以在使用
smbpasswd -n 把用户的密码设置成空. 注意如果设置了"NO PASSWORD"之后，要允许使用者以空口令登入到Samba服务器，管理员必须在smb.conf配置档案的[global]段中设置以下的参数：null passwords = yes
smbpasswd -x 删除用户

service smb status
service smb stop
service smb start
service smb restart
-- 设置Samba服务开机自启动
chkconfig --list | grep smb
chkconfig --level 35 smb on
chkconfig --level 35 nmb on
-- 配置防火墙
-A INPUT -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 137 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 138 -j ACCEPT
```

在window下面
```
sql
\\IP\目录
账号，密码
```
