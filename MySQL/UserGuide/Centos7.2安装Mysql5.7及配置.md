
> 在Linux环境中安装MySQL5.7,Mysql是Web开发最常用的数据库，对于Window开发用户来说都是可视化的安装界面，但是到项目上线时不得不面对Linux环境下的Mysql。所以在Linux环境下安装Mysql，纯命令行的东西还是有些不太适应，笔者这里就介绍一下在Linux环境下搭建Mysql部署环境。

> 前提:已经有一台Linux操作系统，切使用root用户登录
    有wget和vim命令
    
### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：study

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.200

### 查看环境

```
[root@study ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@study ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 安装MySQL5.7


[1] 下载源

```
Centos7默认的yum源中的MySQL为新版本，mariadb
这里要安装的为5.7的版本的MySQL

[root@study ~]# wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm

yum localinstall -y mysql57-community-release-el7-7.noarch.rpm--2018-01-10 00:20:36--  http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
......
100%[===========================================================================>] 8,984       --.-K/s   in 0.004s  

2018-01-10 00:20:39 (2.43 MB/s) - ‘mysql57-community-release-el7-7.noarch.rpm’ saved [8984/8984]
```

[2] 安装Mysql5.7的源

```
[root@study ~]# yum localinstall -y mysql57-community-release-el7-7.noarch.rpm

Loaded plugins: fastestmirror
......

Installed:
  mysql57-community-release.noarch 0:el7-7                                                                           
Complete!
[root@RDFmaster ~]#
```

[3] 安装MySQL

```
[root@study ~]# yum install -y mysql-community-server

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
---> Package mysql-community-server.x86_64 0:5.7.20-1.el7 will be installed
--> Processing Dependency: mysql-community-common(x86-64) = 5.7.20-1.el7 for package: mysql-community-server-5.7.20-1.el7.x86_64
--> Processing Dependency: mysql-community-client(x86-64) >= 5.7.9 for package: mysql-community-server-5.7.20-1.el7.x86_64
.........                 

Replaced:
  mariadb-libs.x86_64 1:5.5.44-2.el7.centos                                                                                  

Complete!

等待安装完成
```

[4] 启动MySQL

```
[root@study ~]# systemctl start mysqld.service

[root@study ~]# systemctl status mysqld.service

● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2018-01-03 16:04:56 CST; 11s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 16060 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 15979 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 16062 (mysqld)
   Memory: 323.0M
   CGroup: /system.slice/mysqld.service
           └─16062 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

Jan 03 16:04:42 study systemd[1]: Starting MySQL Server...
Jan 03 16:04:56 study systemd[1]: Started MySQL Server.
```

[5] 修改密码

```
查看初始密码
[root@study ~]# grep 'temporary password' /var/log/mysqld.log
2018-01-03T08:04:50.026099Z 1 [Note] A temporary password is generated for root@localhost: wIE&vbqyp5rv

[root@study ~]# mysql -u root -p 
Enter password: 输入wIE&vbqyp5rv
mysql> use mysql;
mysql> update user set authentication_string=PASSWORD('newpass') where User='root';
mysql> flush privileges;
mysql> exit

```

[6] 验证

```
[root@study ~]# mysql -u root -p
Enter password: 输入newpass
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.20 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

> 修改好初始密码就可以使用了

### Mysql卸载

- 停止服务

```
[root@study ~]# systemctl stop mysqld.service
```

- 卸载Mysql
 
```
[root@study ~]# yum -e -y mysql-community-server
等待卸载完成
```