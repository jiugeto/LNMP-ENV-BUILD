# Linux-ssh配置

设置其他用户ssh登陆
```
sql
useradd uname
passwd uname 
newpwd
cp /etc/skel/.bash_logout /home/uname/
cp /etc/skel/.bash_profile /home/uname/
cp /etc/skel/.bashrc /home/uname/
su uname
ssh-keygen -t rsa
newpwd
cd /home/uname/.ssh/
sz id_rsa
sz id_rsa.pub
-- 在win7上ssh客户端，身份验证》公匙》属性》会话公匙》输入公匙链接
用户名uname
密码newpwd
-- 开启公匙验证
vi /etc/ssh/sshd_config
PubkeyAuthentication yes        --51行启用
service sshd restart            --重启sshd
-- 要操作最高权限
su 
-- 输入root密码
-- 禁用root登陆
vi /etc/ssh/sshd_config
PermitRootLogin no            --146行禁用root登陆，yes改为no
service sshd restart            --重启sshd
```
