### 前提环境

> 使用上节中已经安装好的PostgreSQL服务，详见[Centos7.2安装PostGreSQL9.5](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85PostGreSQL9.5.md)

> 使用上节配置好的PostgreSQL服务，详见[PostGreSQL9.5的简单配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/PostGreSQL9.5%E7%9A%84%E7%AE%80%E5%8D%95%E9%85%8D%E7%BD%AE.md)

> 使用安装了PostGis的机器，详见[Centos7.2安装部署PostGis2.4.4](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2PostGis2.4.4.md)

> 本节紧接上一节之后进行操作

### 安装软件

[1] 安装使用软件

 1. 安装git

```
[root@demo ~]# yum install git -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
......
Installed:
  git.x86_64 0:1.8.3.1-12.el7_4                                                                                                    

Complete!
```

 2. 安装wget

```
[root@demo ~]# yum install wget -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
......
Installed:
  wget.x86_64 0:1.14-15.el7_4.1                                                                                             

Complete!
```

 3. 安装unzip

```
[root@demo tmp]# yum install unzip -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
......
Installed:
  unzip.x86_64 0:6.0-16.el7                                                      

Complete!
```

[2] 下载osm2pgsql 源码编译安装

 1. 下载源码

```
[root@demo ~]# wget https://github.com/openstreetmap/osm2pgsql/archive/0.94.0.tar.gz
```

 2. 解压

```
[root@demo ~]# tar -zxf 0.94.0.tar.gz
```

 3. 安装编译依赖

```
[root@demo ~]# yum install make cmake gcc-c++ boost-devel expat-devel zlib-devel bzip2-devel postgresql-devel postgresql95-devel geos-devel proj-devel proj-epsg lua-devel -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
......
Installed:
  boost-devel.x86_64 0:1.53.0-27.el7      bzip2-devel.x86_64 0:1.0.6-13.el7            cmake.x86_64 0:2.8.12.2-2.el7      
  expat-devel.x86_64 0:2.1.0-10.el7_3     gcc-c++.x86_64 0:4.8.5-16.el7_4.2            geos-devel.x86_64 0:3.4.2-2.el7    
  lua-devel.x86_64 0:5.1.4-15.el7         postgresql-devel.x86_64 0:9.2.23-3.el7_4     proj-devel.x86_64 0:4.8.0-4.el7    
  proj-epsg.x86_64 0:4.8.0-4.el7          zlib-devel.x86_64 0:1.2.7-17.el7                  
......
Complete!
```

> 注意点:安装postgresql95-devel 这个根据自己安装的PostgreSQL的版本进行安装

```
[root@demo ~]# yum install postgresql95-devel -y
```

 4. 编译前检查,创建构建目录

```
[root@demo ~]# cd osm2pgsql-0.94.0/

[root@demo osm2pgsql-0.94.0]# mkdir build && cd build

[root@demo build]# cmake ..

-- Building osm2pgsql 0.94.0
......
-- Configuring done
-- Generating done
-- Build files have been written to: /root/osm2pgsql-0.94.0/build

```

 5. 检查没有错误,就可以编译安装了

```
[root@demo build]# make && make install

Scanning dependencies of target osm2pgsql_lib
[  3%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/expire-tiles.cpp.o
......
[100%] Building CXX object CMakeFiles/osm2pgsql.dir/osm2pgsql.cpp.o
Linking CXX executable osm2pgsql
[100%] Built target osm2pgsql
[ 96%] Built target osm2pgsql_lib
[100%] Built target osm2pgsql
Install the project...
-- Install configuration: "RelWithDebInfo"
-- Installing: /usr/local/bin/osm2pgsql
-- Removed runtime path from "/usr/local/bin/osm2pgsql"
-- Installing: /usr/local/share/man/man1/osm2pgsql.1
-- Installing: /usr/local/share/osm2pgsql/default.style
-- Installing: /usr/local/share/osm2pgsql/empty.style
```

 6. 查看安装情况

```
[root@demo build]# osm2pgsql -version

osm2pgsql version 0.94.0 (64 bit id space)

Osm2pgsql failed due to ERROR: Missing zoom level for tile expiry.
```

> 输出有一个ERROR是因为缩放级别的问题,不影响使用,至此osm2pgsql安装完毕.


















 1. 数据准备

```
openstreetmap 数据下载页面http://download.geofabrik.de/asia.html

[root@demo build]# cd /tmp

[root@demo tmp]# wget http://download.geofabrik.de/asia/china-latest.osm.pbf

[root@demo tmp]# chmod 755 china-latest.osm.pbf
```

 2. 数据导入

```
[root@demo tmp]# su postgres

bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf

osm2pgsql version 0.94.0 (64 bit id space)

Password: 12345678
Using built-in tag processing pipeline
......
Reading in file: /tmp/china-latest.osm.pbf
Using PBF parser.
Processing: Node(55739k 224.8k/s) Way(4070k 26.60k/s) Relation(45850 487.77/s)  parse time: 495s
Node stats: total(55739733), max(5577202979) in 248s
Way stats: total(4070437), max(583356532) in 153s
Relation stats: total(45899), max(8244324) in 94s
......
node cache: stored: 52910936(94.92%), storage efficiency: 50.46% (dense blocks: 426, sparse nodes: 50683905), hit rate: 94.94%

Osm2pgsql took 2152s overall

bash-4.2$ exit
```

[4] openstreetmap-carto样式导入

 1. 下载样式

```
[root@demo tmp]# wget -O openstreetmap-carto-master.zip https://codeload.github.com/gravitystorm/openstreetmap-carto/zip/master

......
100%[=======================================>] 2,907,152    161KB/s   in 23s    

2018-04-26 19:11:12 (124 KB/s) - ‘openstreetmap-carto-master.zip’ saved [2907152/2907152]
```

 2. 解压

```
[root@demo tmp]# unzip openstreetmap-carto-master.zip 

  inflating: openstreetmap-carto-master/water.mss  
finishing deferred symbolic links:
  openstreetmap-carto-master/scripts/lua/openstreetmap-carto.lua -> ../../openstreetmap-carto.lua
```

 3. 移动文件

```
[root@demo tmp]# mv openstreetmap-carto-master /home/postgresql_data/
```

 4. 导入

```
[root@demo tmp]# su postgres

bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf --style /home/postgresql_data/openstreetmap-carto-master/openstreetmap-carto.style

osm2pgsql version 0.94.0 (64 bit id space)

Password: 12345678
Using built-in tag processing pipeline
......
Using PBF parser.
Processing: Node(55739k 192.2k/s) Way(4070k 25.76k/s) Relation(45730 476.35/s)  parse time: 544s
Node stats: total(55739733), max(5577202979) in 290s
Way stats: total(4070437), max(583356532) in 158s
Relation stats: total(45899), max(8244324) in 96s
Committing transaction for planet_osm_point
......
node cache: stored: 52910936(94.92%), storage efficiency: 50.46% (dense blocks: 426, sparse nodes: 50683905), hit rate: 94.94%

Osm2pgsql took 2346s overall
```

> 数据导入完成archive/0.94.0.tar.gz

......
Saving to: ‘0.94.0.tar.gz’

100%[==================================================================================>] 1,203,310    144KB/s   in 9.8s   

2018-04-26 17:21:12 (120 KB/s) - ‘0.94.0.tar.gz’ saved [1203310/1203310]
```

