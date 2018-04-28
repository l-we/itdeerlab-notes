### 前提环境

> 使用上节中已经安装好的PostgreSQL服务，详见[Centos7.2安装PostGreSQL9.5](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85PostGreSQL9.5.md)

### PostgreSQL9.5的简单使用

[1] 创建名称为“demo”的库，并查看(紧接上一节之后)

```
postgres=# CREATE DATABASE demo;
CREATE DATABASE

postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 demo      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(4 rows)

```

[2] 创建名称为“demo”的用户并设置密码，拥有登录和创建库的权限

```
postgres=# CREATE USER demo CREATEDB LOGIN PASSWORD '12345678';
CREATE ROLE
```

[3] 把demo的库授权给demo用户

```
postgres=# GRANT ALL ON DATABASE demo TO demo;
GRANT
postgres=# \q
```

[4] 验证使用demo用户登录

```
bash-4.2$ psql -U demo -h 127.0.0.1 -p 5432 -d demo -W
could not change directory to "/root": Permission denied
Password for user demo: 
psql: FATAL:  Ident authentication failed for user "demo"
```

> 登录失败，身份验证失败

[5] 更改配置(配置在数据存储目录下,上节配置的/home/prostgresql/目录下)

```
bash-4.2$ vim /home/postgresql_data/postgresql.conf

找到 #listen_addresses = 'localhost'         # what IP address(es) to listen on;
更为 listen_addresses = '*'         # what IP address(es) to listen on;

找到 #port = 5432 							# (change requires restart)
改为 port = 5432 							# (change requires restart)

找到 #password_encryption = on
改为 password_encryption = on
```

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-2.png)

```
bash-4.2$ vim /home/postgresql_data/pg_hba.conf

找到 host    all             all             127.0.0.1/32            ident
改为 host    all             all             127.0.0.1/32            md5

在最下面添加 host    all             all             0.0.0.0/0                md5
```

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-3.png)

[6] 重启服务

```
bash-4.2$ exit
[root@demo ~]# systemctl restart postgresql-9.5.service
```

[7] 登录验证

```
[root@demo ~]# su postgres

bash-4.2$ psql -U demo -h 127.0.0.1 -p 5432 -d demo -W

could not change directory to "/root": Permission denied
Password for user demo: 12345678
psql (9.5.12)
Type "help" for help.

demo=>\q

bash-4.2$ exit
```

> 以上的配置之后可以使用demo 用户登录Postgresql系统中做操作。