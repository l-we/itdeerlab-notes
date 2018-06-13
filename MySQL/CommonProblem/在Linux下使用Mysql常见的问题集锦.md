
> 在Linux环境下运维Mysql经常会遇到一些问题，有的比较简单，但是每一次都是需要上网查询，也是比较麻烦的，这里我帮大家总结一些，希望对你有所帮助。

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

### 问题一：忘记root密码

- 修改配置文件让其跳过密码验证

```
[root@study ~]# vim /etc/my.cnf
在配置文件中添加skip-grant-tables
[mysqld]
skip-grant-tables

保存退出
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```

- 修改密码

```
[root@study ~]# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.20 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

mysql> update user set authentication_string=PASSWORD('newpass') where User='root';
Query OK, 0 rows affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 1

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
```

- 把配置文件修改回来

```
[root@study ~]# vim /etc/my.cnf
在配置文件中添加skip-grant-tables去掉
[mysqld]

保存退出
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```

- 验证

```
[root@study ~]# mysql -u root -p
Enter password：输入newpass
```

### 问题二：创建指定编码的数据库

- 创建UTF-8编码的数据库

```
[root@study ~]# mysql -u root -p
Enter password：输入newpass
mysql> CREATE DATABASE blog DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci; 
Query OK, 1 row affected (0.00 sec)

mysql> use blog;
Database changed
```
- 创建GBK编码的数据库

```
[root@study ~]# mysql -u root -p
Enter password：输入newpass
mysql> CREATE DATABASE blog DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
Query OK, 1 row affected (0.00 sec)

mysql> use blog;
Database changed
```

### 问题三：数据库数据的导入导出

- 数据导入

> 首先blog.sql文件已经放到/opt/目录下，且blog.sql文件中有数据和建表语句

```
mysql> use blog;
mysql> source /opt/blog.sql
等待导入完成即可
```

- 数据导出

```
[root@study ~]# mysqldump -u root -p blog > /opt/blog_bak.sql
Enter password: 输入密码
[root@study ~]# ll /opt/
total 4
-rw-r--r-- 1 root root 1245 Jan  3 16:53 blog_bak.sql
```

### 问题四：修改密码

- 方法一

> 在mysql系统外，使用mysqladmin

```
[root@study ~]# mysqladmin -u root -p password "root"
Enter password: 【输入原来的密码】
```
- 方法二

> 通过登录mysql系统

```
[root@study ~]# mysql -uroot -p
Enter password: 【输入原来的密码】
mysql>use mysql;
mysql> update user set authentication_string=PASSWORD('newpass') where User='root';
mysql> flush privileges;
mysql> exit;
```
  
### 问题五：Mysql区分大小写

> 在Linux环境下Mysql安装完成之后默认是区分大小写的，在部署应用的时候会遇到找不到表的时候，
 出现问题，这时候可能就是大小写的问题。在Window环境下，Mysql一直都是不区分大小写的。
 
 - 修改配置

```
[root@study ~]# vim /etc/my.cnf
添加一句lower_case_table_names=1
[mysqld]
lower_case_table_names=1
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```
- 查看参数

```
[root@study ~]# mysql -u root -p
Enter password：输入密码
mysql> show variables like "%case%" ;
+------------------------------------+-------+
| Variable_name                      | Value |
+------------------------------------+-------+
| lower_case_file_system             | OFF   |
| lower_case_table_names             | 0     |
| validate_password_mixed_case_count | 1     |
+------------------------------------+-------+
3 rows in set (0.00 sec)
```

### 问题六：Mysql乱码问题

> Mysql乱码是Mysql服务折腾程序员的最大的一个问题，尤其是新手对这个问题是一直挠头没有办法。
其实乱码，无非就是因为有一个地方编码不统一了，所以就乱了。保证程序的编码和数据库的编码一致,就不会出现这个问题了。

- 查看默认编码

```
mysql> show variables like '%character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | latin1                       |
| character_set_connection | latin1                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | latin1                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

- 修改配置

```
[root@study ~]# vim /etc/my.cnf
修改添加以下三句话，若[client]没有，自己添加即可
[client]
default-character-set=utf8

[mysqld]
character-set-server=utf8
init_connect='SET NAMES utf8'
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```

- 验证

```
mysql> show variables like '%character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

> 值得注意的是，在创建数据库的时候尽量指明默认字符集为utf8

### 问题七：Mysql最大连接上限

- 查询当前 set GLOBAL max_connections=1000;


```
mysql> SELECT @@MAX_CONNECTIONS AS 'Max Connections';
+-----------------+
| Max Connections |
+-----------------+
|             151 |
+-----------------+
1 row in set (0.00 sec)
```

- 临时设置 set GLOBAL max_connections=1000;

```
mysql>  set GLOBAL max_connections=1000;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @@MAX_CONNECTIONS AS 'Max Connections';
+-----------------+
| Max Connections |
+-----------------+
|            1000 |
+-----------------+
1 row in set (0.00 sec)
```

- 永久设置

```
[root@study ~]# vim /etc/my.cnf
添加一行max_connections = 1200
[mysqld]
max_connections = 1200
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```
- 其他查询

> 显示当前运行的Query

```
mysql> show processlist;
+----+------+-----------+------+---------+------+----------+------------------+
| Id | User | Host      | db   | Command | Time | State    | Info             |
+----+------+-----------+------+---------+------+----------+------------------+
|  3 | root | localhost | NULL | Query   |    0 | starting | show processlist |
+----+------+-----------+------+---------+------+----------+------------------+
1 row in set (0.00 sec)
```

> 如何查询mysql的已连接数

```
mysql> show full processlist;
+----+------+-----------+------+---------+------+----------+-----------------------+
| Id | User | Host      | db   | Command | Time | State    | Info                  |
+----+------+-----------+------+---------+------+----------+-----------------------+
|  3 | root | localhost | NULL | Query   |    0 | starting | show full processlist |
+----+------+-----------+------+---------+------+----------+-----------------------+
1 row in set (0.00 sec)
```

### 问题八：删除数据库失败

```
mysql> drop database hive;
ERROR 1010 (HY000): Error dropping database (can't rmdir './hive',errno:39)

进入/var/lib/mysql/  删除hive这个文件就可以了
```
> Mysql在Linux服务器上使用的问题还是有很多的。后续还会出更多的问题场景及解决方式


### 问题九：打开root远程连接

> Linux操作系统上的Mysql数据库，但是在开发过程中总是希望能使用客户端工具远程连接进入，但是发现Mysql默认不能远程连接

```
[root@study ~]# mysql -u root -p
Enter password: Itdeer123$%^
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.21 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.



mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Itdeer123$%^' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)


mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
```

- 重启服务

```
[root@study ~]# systemctl restart mysqld
```

- 然后使用Window端的客户端即可连接到服务器端的Mysql服务。