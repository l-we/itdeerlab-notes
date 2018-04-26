### 前提环境

> 使用上节中已经安装好的PostgreSQL服务，详见[Centos7.2安装PostGreSQL9.5](http://note.youdao.com/)

> 使用上节配置好的PostgreSQL服务，详见[PostGreSQL9.5的简单配置](http://note.youdao.com/)

### 安装软件

[1] 安装epel源

```
[root@demo ~]# yum install epel-release -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.neusoft.edu.cn
 * extras: mirror.bit.edu.cn
 * updates: mirror.bit.edu.cn

......

Running transaction
  Installing : epel-release-7-9.noarch                                                                                1/1 
  Verifying  : epel-release-7-9.noarch                                                                                1/1 

Installed:
  epel-release.noarch 0:7-9                                                                                               

Complete!
```

[2] 安装PostGis

```
[root@demo ~]# yum install postgis2_95 postgis2_95-client -y
Loaded plugins: fastestmirror
epel/x86_64/metalink                                                                               | 7.8 kB  00:00:00     
epel                                                                                               | 4.7 kB  00:00:00     
(1/3): epel/x86_64/group_gz                                                                        | 266 kB  00:00:00     
(2/3): epel/x86_64/updateinfo                                                                      | 917 kB  00:00:01     
(3/3): epel/x86_64/primary_db                                                                      | 6.3 MB  00:00:09     
Loading mirror speeds from cached hostfile

......

  Verifying  : glib2-2.42.2-5.el7.x86_64                                                                            72/72 

Installed:
  postgis24_95.x86_64 0:2.4.4-1.rhel7                      postgis24_95-client.x86_64 0:2.4.4-1.rhel7                     

Dependency Installed:

......

Dependency Updated:
  glib2.x86_64 0:2.50.3-3.el7                                                                                             

Complete!
```

[3] 安装ogrfdw

```
[root@demo ~]# yum install ogr_fdw95 -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.neusoft.edu.cn
 * epel: mirrors.tongji.edu.cn

......

Installed:
  ogr_fdw95.x86_64 0:1.0.4-1.rhel7                                                                                        

Complete!
```

[4] 安装pgRouting

```
[root@demo ~]# yum install pgrouting_95 -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.neusoft.edu.cn
 * epel: mirrors.tongji.edu.cn

......

Installed:
  pgrouting_95.x86_64 0:2.6.0-1.rhel7                                                                                     

Dependency Installed:
  boost-atomic.x86_64 0:1.53.0-27.el7                         boost-chrono.x86_64 0:1.53.0-27.el7                        

Complete!

```

[5] 安装PostGis扩展

 - 创建名为 demogisdb 的库 并查看

```
[root@demo ~]# su postgres
bash-4.2$ psql
could not change directory to "/root": Permission denied
psql (9.5.12)
Type "help" for help.

postgres=# CREATE DATABASE demogisdb OWNER demo;
CREATE DATABASE
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 demo      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =Tc/postgres         +
           |          |          |             |             | postgres=CTc/postgres+
           |          |          |             |             | demo=CTc/postgres
 demogisdb | demo     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(5 rows)

```

 - 切换到 demogisdb 库，安装PostGis的扩展

```
postgres=# \c demogisdb
You are now connected to database "demogisdb" as user "postgres".

demogisdb=# CREATE EXTENSION postgis;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION postgis_topology;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION ogr_fdw;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION postgis_sfcgal;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION fuzzystrmatch;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION address_standardizer;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION address_standardizer_data_us;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION postgis_tiger_geocoder;
CREATE EXTENSION
demogisdb=# CREATE EXTENSION hstore;
CREATE EXTENSION


demogisdb=# SELECT postgis_full_version();
                                                                                            postgis_full_version          
                                                                                  
--------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------
 POSTGIS="2.4.4 r16526" PGSQL="95" GEOS="3.6.2-CAPI-1.10.2 4d2925d6" SFCGAL="1.2.2" PROJ="Rel. 4.9.3, 15 August 2016" GDAL
="GDAL 1.11.4, released 2016/01/25" LIBXML="2.9.1" LIBJSON="0.11" TOPOLOGY RASTER
(1 row)

demogisdb=# 
```

> 显示版本号为 POSTGIS="2.4.4 r16526" 说明PostGis安装完成。 
