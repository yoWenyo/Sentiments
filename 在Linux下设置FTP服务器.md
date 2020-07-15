# 在Linux下设置FTP服务器

## 需求背景

> &emsp;因为计算机网络作业需要，我需要FTP传输过程中的豹纹记录，所以在自己的虚拟机里面开启放FTP服务，并用另一台虚拟机尝试访问此服务器。

## 问题与解决

> 刚开始跟着网站的帖子走，安装vsftpd，开启服务，然后尝试了很多没有连接成功，原因是：vsftpd的登录登录账户为linux的账户，当然我看到帖子上介绍存在userlist的相关选项，还有vsusserlist的目录，知道ftp的应用应该还有用户配置选项，但是应该是升级了，可以直接使用linux用户名密码直接登录（也或者本来就是能用linux的用户名密码登录）。  

## 安装记录  

```c
//安装vsftpd
sudo apt-get install vsftpd

//开启放FTP服务
service vsftpd start

//查看FTP服务器状态
service vsftpd status

//重启FTP服务器
service vsftpd restart

//查看是否开启服务(没用到)
netstat -an | grep 21

//开启用户列表的，我没用到
userlist_enable=YES     #启动用户列表
userlist_deny=NO        #决定是否对用户列表的用户拒绝访问ftp 

userlist_file=/etc/vsftpd/user_list
```
