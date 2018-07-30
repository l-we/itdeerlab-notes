### 简要说明

> 对于软件的安装，其实是很简单的，无非就是一些命令，等着就行，之后做一些简单的配置，就可以使用了。对于简单的使用者来说，足够了。但是这样的操作，时间一长就会忘记很多，我一直认同“好记忆，不如烂笔头”，刚好现在有时间，做一个记录，也方便以后查找，同时能帮助到需要的朋友就更加完美了。闲话就不说了，步入正题。


> PostgreSQL 上手没有MySQL简单（MySQL做深了也是很难的），PostgreSQL 之前的版本，你叫它是一个关系型数据库吧，是称得上的，但是PostgreSQL9.3就内置了JSON数据类型，PostgreSQL9.4又支持了JSONB，这PostgreSQL不能称得上是一个关系数据库了，算是一个关系型数据库和NoSQL数据库的结合体。同时PostgreSQL PostGis又很好的支持空间数据存储和空间分析，这是它的又一个优势，下面就介绍一下PostgreSQL在CentOS7.2上的安装使用。


### 环境准备

 - CentOS7.2.1511操作系统
 - 系统语言为英文
 - 系统最小化安装
 - Linux内核版本3.10.0
 - 使用的用户：root
 - 登录密码为：123456
 - 主机名称为：demo

### 系统检查

```
[root@demo ~]# uname -a
Linux demo 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

[root@demo ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```

### 软件安装

> CentOS7.2中自带了PostgreSQL9.2版本，这里不使用自带的版本，我们安装PostgreSQL9.5的版本。

[1] 安装PostgreSQL的RPM包

```
[root@demo ~]# yum install http://yum.postgresql.org/9.5/redhat/rhel-7-x86_64/pgdg-redhat95-9.5-2.noarch.rpm -y

Loaded plugins: fastestmirror
Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
pgdg-redhat95-9.5-2.noarch.rpm                     | 5.3 kB     00:00     
......
Running transaction
  Installing : pgdg-redhat95-9.5-2.noarch                             1/1 
  Verifying  : pgdg-redhat95-9.5-2.noarch                             1/1 

Installed:
  pgdg-redhat95.noarch 0:9.5-2                                            

Complete!
```

[2] 安装PostgreSQL的服务

```
[root@demo ~]# yum install postgresql95-server postgresql95-contrib -y

Loaded plugins: fastestmirror
base                                               | 3.6 kB     00:00     
extras                                             | 3.4 kB     00:00     
pgdg95                                             | 4.1 kB     00:00     

......

Installed:
  postgresql95-contrib.x86_64 0:9.5.12-1PGDG.rhel7                        
  postgresql95-server.x86_64 0:9.5.12-1PGDG.rhel7                         

Dependency Installed:
  libxslt.x86_64 0:1.1.28-5.el7                                           
  postgresql95.x86_64 0:9.5.12-1PGDG.rhel7                                
  postgresql95-libs.x86_64 0:9.5.12-1PGDG.rhel7                           

Complete!
```

[3] 更改配置

> PostgreSQL默认安装完成之后，数据存储目录是在/var/lib/pgsql/[PostgreSQL的版本号]/data/目录下。这个数据目录下的数据所占存储是系统的根目录，若之前把/var/目录配置的很大也是可以的，但是一般系统的磁盘空间都不是很大，所以这里要做一下更改。有两种方式：

 - 第一种方式是：挂载一个大的数据盘到PostgreSQL的data目录，这种方式较简单。

 - 第二种方式是：更改PostgreSQL的数据存储目录，这种方式较复杂些。

> 这里使用第二种方式，做一下配置，看一下本系统的存储配置信息

```
[root@demo ~]# df -h

Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   99G  1.1G   98G   2% /
devtmpfs                 4.8G     0  4.8G   0% /dev
tmpfs                    4.9G     0  4.9G   0% /dev/shm
tmpfs                    4.9G  8.5M  4.8G   1% /run
tmpfs                    4.9G     0  4.9G   0% /sys/fs/cgroup
/dev/mapper/centos-home   91G   33M   91G   1% /home
/dev/sda1                497M  125M  373M  25% /boot
tmpfs                    984M     0  984M   0% /run/user/0
```

> 能看到/home目录下较大，把数据目录放到/home目录下,这个根据自己的情况进行更改

 - 创建数据目录

```
[root@demo ~]# mkdir -p /home/postgresql_data
```

 - 更改数据目录所属组、所属主

```
[root@demo ~]# chown postgres:postgres /home/postgresql_data/
```

 - 更改数据目录权限

```
[root@demo ~]# chmod 700 /home/postgresql_data/

[root@demo ~]# ll /home/
total 0
drwx------. 2 postgres postgres 6 Apr 25 23:50 postgresql_data
```

 - 更改PostgreSQL的数据目录(/var/lib/pgsql/[PostgreSQL的版本号]/data/)配置指向上面创建的目录

```
[root@demo ~]# vim /usr/lib/systemd/system/postgresql-9.5.service

找到 Environment=PGDATA=/var/lib/pgsql/9.5/data/
更改 Environment=PGDATA=/home/postgresql_data/

注意：有的时候直接复制过去会出问题，在向文件中写的字符建议自己敲，因为编码格式等原因会不对。
```

![2018621141627](http://note.itdeer.cn/2018621141627.png)

 - 初始化数据库

```
[root@demo ~]# /usr/pgsql-9.5/bin/postgresql95-setup initdb

Initializing database ... OK
```

 - 查看执行日志

```
[root@demo 9.5]# cat /var/lib/pgsql/9.5/initdb.log

The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

......

copying template1 to template0 ... ok
copying template1 to postgres ... ok
syncing data to disk ... ok

Success. You can now start the database server using:

    /usr/pgsql-9.5/bin/pg_ctl -D /home/postgresql_data/ -l logfile start
```

### 结果验证

[1] 启动服务

 - 启动服务

```
[root@demo ~]# systemctl start postgresql-9.5.service
```

 - 服务状态
 
```
[root@demo ~]# systemctl status postgresql-9.5.service

?.postgresql-9.5.service - PostgreSQL 9.5 database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql-9.5.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2018-04-26 00:19:20 CST; 10s ago
     Docs: https://www.postgresql.org/docs/9.5/static/
  Process: 2901 ExecStart=/usr/pgsql-9.5/bin/pg_ctl start -D ${PGDATA} -s -w -t 300 (code=exited, status=0/SUCCESS)
  Process: 2895 ExecStartPre=/usr/pgsql-9.5/bin/postgresql95-check-db-dir ${PGDATA} (code=exited, status=0/SUCCESS)

......

Apr 26 00:19:19 demo pg_ctl[2901]: < 2018-04-26 00:19:19.229 CST >HINT....
Apr 26 00:19:20 demo systemd[1]: Started PostgreSQL 9.5 database server.
Hint: Some lines were ellipsized, use -l to show in full.
```

[2] 开机启动

```
[root@demo ~]# systemctl enable postgresql-9.5.service

Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql-9.5.service to /usr/lib/systemd/system/postgresql-9.5.service.
```

[3] 更改postgres用户密码(PostgreSQL安装之后默认的用户为postgres)

```
[root@demo ~]# passwd postgres
Changing password for user postgres.
New password: 12345678
BAD PASSWORD: The password fails the dictionary check - it is too simplistic/systematic
Retype new password: 12345678
passwd: all authentication tokens updated successfully.
```

[4] 登录数据库

```
[root@demo ~]# su postgres

bash-4.2$ psql
could not change directory to "/root": Permission denied
psql (9.5.12)
Type "help" for help.

postgres=# 
```

[5] 列出数据信息

```
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)

postgres=#
```

> 至此PostgreSQL9.5在CentOS7.2.1511安装完成