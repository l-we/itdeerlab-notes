

### 环境准备




### 安装软件

[1] 安装使用软件

- git

```
[root@demo ~]# yum install git -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirror.bit.edu.cn
Resolving Dependencies
--> Running transaction check
---> Package git.x86_64 0:1.8.3.1-12.el7_4 will be installed
--> Processing Dependency: perl-Git = 1.8.3.1-12.el7_4 for package: git-1.8.3.1-12.el7_4.x86_64
--> Processing Dependency: rsync for package: git-1.8.3.1-12.el7_4.x86_64
--> Processing Dependency: perl(Term::ReadKey) for package: git-1.8.3.1-12.el7_4.x86_64
--> Processing Dependency: perl(Git) for package: git-1.8.3.1-12.el7_4.x86_64
--> Processing Dependency: perl(Error) for package: git-1.8.3.1-12.el7_4.x86_64
--> Processing Dependency: libgnome-keyring.so.0()(64bit) for package: git-1.8.3.1-12.el7_4.x86_64
--> Running transaction check
---> Package libgnome-keyring.x86_64 0:3.12.0-1.el7 will be installed
---> Package perl-Error.noarch 1:0.17020-2.el7 will be installed
---> Package perl-Git.noarch 0:1.8.3.1-12.el7_4 will be installed
---> Package perl-TermReadKey.x86_64 0:2.30-20.el7 will be installed
---> Package rsync.x86_64 0:3.0.9-18.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

============================================================================================================================
 Package                           Arch                    Version                           Repository                Size
============================================================================================================================
Installing:
 git                               x86_64                  1.8.3.1-12.el7_4                  updates                  4.4 M
Installing for dependencies:
 libgnome-keyring                  x86_64                  3.12.0-1.el7                      base                     109 k
 perl-Error                        noarch                  1:0.17020-2.el7                   base                      32 k
 perl-Git                          noarch                  1.8.3.1-12.el7_4                  updates                   53 k
 perl-TermReadKey                  x86_64                  2.30-20.el7                       base                      31 k
 rsync                             x86_64                  3.0.9-18.el7                      base                     360 k

Transaction Summary
============================================================================================================================
Install  1 Package (+5 Dependent packages)

Total download size: 5.0 M
Installed size: 23 M
Downloading packages:
(1/6): perl-Error-0.17020-2.el7.noarch.rpm                                                           |  32 kB  00:00:00     
(2/6): perl-Git-1.8.3.1-12.el7_4.noarch.rpm                                                          |  53 kB  00:00:00     
(3/6): libgnome-keyring-3.12.0-1.el7.x86_64.rpm                                                      | 109 kB  00:00:00     
(4/6): perl-TermReadKey-2.30-20.el7.x86_64.rpm                                                       |  31 kB  00:00:00     
(5/6): rsync-3.0.9-18.el7.x86_64.rpm                                                                 | 360 kB  00:00:00     
(6/6): git-1.8.3.1-12.el7_4.x86_64.rpm                                                               | 4.4 MB  00:00:00     
----------------------------------------------------------------------------------------------------------------------------
Total                                                                                       5.1 MB/s | 5.0 MB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : 1:perl-Error-0.17020-2.el7.noarch                                                                        1/6 
  Installing : perl-TermReadKey-2.30-20.el7.x86_64                                                                      2/6 
  Installing : libgnome-keyring-3.12.0-1.el7.x86_64                                                                     3/6 
  Installing : rsync-3.0.9-18.el7.x86_64                                                                                4/6 
  Installing : perl-Git-1.8.3.1-12.el7_4.noarch                                                                         5/6 
  Installing : git-1.8.3.1-12.el7_4.x86_64                                                                              6/6 
  Verifying  : rsync-3.0.9-18.el7.x86_64                                                                                1/6 
  Verifying  : libgnome-keyring-3.12.0-1.el7.x86_64                                                                     2/6 
  Verifying  : perl-Git-1.8.3.1-12.el7_4.noarch                                                                         3/6 
  Verifying  : perl-TermReadKey-2.30-20.el7.x86_64                                                                      4/6 
  Verifying  : 1:perl-Error-0.17020-2.el7.noarch                                                                        5/6 
  Verifying  : git-1.8.3.1-12.el7_4.x86_64                                                                              6/6 

Installed:
  git.x86_64 0:1.8.3.1-12.el7_4                                                                                             

Dependency Installed:
  libgnome-keyring.x86_64 0:3.12.0-1.el7      perl-Error.noarch 1:0.17020-2.el7      perl-Git.noarch 0:1.8.3.1-12.el7_4     
  perl-TermReadKey.x86_64 0:2.30-20.el7       rsync.x86_64 0:3.0.9-18.el7           

Complete!
```

 - 安装wget

```
[root@demo ~]# yum install wget -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirror.bit.edu.cn
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-15.el7_4.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

============================================================================================================================
 Package                  Arch                       Version                              Repository                   Size
============================================================================================================================
Installing:
 wget                     x86_64                     1.14-15.el7_4.1                      updates                     547 k

Transaction Summary
============================================================================================================================
Install  1 Package

Total download size: 547 k
Installed size: 2.0 M
Downloading packages:
wget-1.14-15.el7_4.1.x86_64.rpm                                                                      | 547 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-15.el7_4.1.x86_64                                                                              1/1 
  Verifying  : wget-1.14-15.el7_4.1.x86_64                                                                              1/1 

Installed:
  wget.x86_64 0:1.14-15.el7_4.1                                                                                             

Complete!
```

 - 安装unzip

```
[root@demo tmp]# yum install unzip -y
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirror.bit.edu.cn
Resolving Dependencies
--> Running transaction check
---> Package unzip.x86_64 0:6.0-16.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=================================================================================
 Package          Arch              Version                Repository       Size
=================================================================================
Installing:
 unzip            x86_64            6.0-16.el7             base            169 k

Transaction Summary
=================================================================================
Install  1 Package

Total download size: 169 k
Installed size: 365 k
Downloading packages:
unzip-6.0-16.el7.x86_64.rpm                               | 169 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : unzip-6.0-16.el7.x86_64                                       1/1 
  Verifying  : unzip-6.0-16.el7.x86_64                                       1/1 

Installed:
  unzip.x86_64 0:6.0-16.el7                                                      

Complete!
```



[3] 下载osm2pgsql 源码编译安装

 - 下载源码

```
[root@demo ~]# wget https://github.com/openstreetmap/osm2pgsql/archive/0.94.0.tar.gz
--2018-04-26 17:21:00--  https://github.com/openstreetmap/osm2pgsql/archive/0.94.0.tar.gz
Resolving github.com (github.com)... 13.250.177.223, 52.74.223.119, 13.229.188.59
Connecting to github.com (github.com)|13.250.177.223|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/openstreetmap/osm2pgsql/tar.gz/0.94.0 [following]
--2018-04-26 17:21:01--  https://codeload.github.com/openstreetmap/osm2pgsql/tar.gz/0.94.0
Resolving codeload.github.com (codeload.github.com)... 13.229.189.0, 13.250.162.133, 54.251.140.56
Connecting to codeload.github.com (codeload.github.com)|13.229.189.0|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1203310 (1.1M) [application/x-gzip]
Saving to: ‘0.94.0.tar.gz’

100%[==================================================================================>] 1,203,310    144KB/s   in 9.8s   

2018-04-26 17:21:12 (120 KB/s) - ‘0.94.0.tar.gz’ saved [1203310/1203310]

```

 - 解压

```
[root@demo ~]# tar -zxf 0.94.0.tar.gz
```

- 安装编译依赖

```
[root@demo ~]# yum install make cmake gcc-c++ boost-devel expat-devel zlib-devel bzip2-devel postgresql-devel postgresql95-devel geos-devel proj-devel proj-epsg lua-devel -y

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.tuna.tsinghua.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirror.bit.edu.cn
Resolving Dependencies

......

Installed:
  boost-devel.x86_64 0:1.53.0-27.el7      bzip2-devel.x86_64 0:1.0.6-13.el7            cmake.x86_64 0:2.8.12.2-2.el7      
  expat-devel.x86_64 0:2.1.0-10.el7_3     gcc-c++.x86_64 0:4.8.5-16.el7_4.2            geos-devel.x86_64 0:3.4.2-2.el7    
  lua-devel.x86_64 0:5.1.4-15.el7         postgresql-devel.x86_64 0:9.2.23-3.el7_4     proj-devel.x86_64 0:4.8.0-4.el7    
  proj-epsg.x86_64 0:4.8.0-4.el7          zlib-devel.x86_64 0:1.2.7-17.el7            

Dependency Installed:
  boost.x86_64 0:1.53.0-27.el7                                 boost-context.x86_64 0:1.53.0-27.el7                         
  boost-filesystem.x86_64 0:1.53.0-27.el7                      boost-graph.x86_64 0:1.53.0-27.el7                           
  boost-iostreams.x86_64 0:1.53.0-27.el7                       boost-locale.x86_64 0:1.53.0-27.el7                          
  boost-math.x86_64 0:1.53.0-27.el7                            boost-program-options.x86_64 0:1.53.0-27.el7                 
  boost-python.x86_64 0:1.53.0-27.el7                          boost-random.x86_64 0:1.53.0-27.el7                          
  boost-regex.x86_64 0:1.53.0-27.el7                           boost-signals.x86_64 0:1.53.0-27.el7                         
  boost-test.x86_64 0:1.53.0-27.el7                            boost-timer.x86_64 0:1.53.0-27.el7                           
  boost-wave.x86_64 0:1.53.0-27.el7                            cpp.x86_64 0:4.8.5-16.el7_4.2                                
  gcc.x86_64 0:4.8.5-16.el7_4.2                                geos.x86_64 0:3.4.2-2.el7                                    
  glibc-devel.x86_64 0:2.17-196.el7_4.2                        glibc-headers.x86_64 0:2.17-196.el7_4.2                      
  kernel-headers.x86_64 0:3.10.0-693.21.1.el7                  libarchive.x86_64 0:3.1.2-10.el7_2                           
  libicu.x86_64 0:50.1.2-15.el7                                libmpc.x86_64 0:1.0.1-3.el7                                  
  libstdc++-devel.x86_64 0:4.8.5-16.el7_4.2                    postgresql.x86_64 0:9.2.23-3.el7_4                           
  postgresql-libs.x86_64 0:9.2.23-3.el7_4                     

Dependency Updated:
  expat.x86_64 0:2.1.0-10.el7_3         glibc.x86_64 0:2.17-196.el7_4.2        glibc-common.x86_64 0:2.17-196.el7_4.2     
  libgcc.x86_64 0:4.8.5-16.el7_4.2      libgomp.x86_64 0:4.8.5-16.el7_4.2      libstdc++.x86_64 0:4.8.5-16.el7_4.2        
  lua.x86_64 0:5.1.4-15.el7             zlib.x86_64 0:1.2.7-17.el7            

Complete!
```

 - 编译前检查

 ```
[root@demo ~]# cd osm2pgsql-0.94.0/
[root@demo osm2pgsql-0.94.0]# mkdir build && cd build

[root@demo build]# cmake ..
-- Building osm2pgsql 0.94.0
-- Boost version: 1.53.0
-- Found the following Boost libraries:
--   system
--   filesystem
-- Found PostgreSQL: /usr/pgsql-9.5/lib (found version "9.5.12") 
-- PostgreSQL include dirs: /usr/pgsql-9.5/include;/usr/include
-- PostgreSQL library dirs: /usr/pgsql-9.5/lib
-- PostgreSQL libraries:    pq
Libraries used to build: /usr/lib64/libboost_system-mt.so/usr/lib64/libboost_filesystem-mt.so/usr/pgsql-9.5/lib/libpq.so/usr/lib64/libz.so-lpthread/usr/lib64/libexpat.so/usr/lib64/libbz2.so/usr/lib64/libproj.so/usr/lib64/liblua-5.1.so/usr/lib64/libm.so
-- Looking for clock_gettime in rt
-- Looking for clock_gettime in rt - found
Active compiler flags:
-- Added test: test-expire-tiles...
-- Added test: test-hstore-match-only...
-- Added test: test-middle-flat...
-- Added test: test-middle-pgsql...
-- Added test: test-middle-ram...
-- Added test: test-options-database...
-- Added test: test-options-parse...
-- Added test: test-options-projection...
-- Added test: test-output-multi-line-storage...
-- Added test: test-output-multi-line...
-- Added test: test-output-multi-point-multi-table...
-- Added test: test-output-multi-point...
-- Added test: test-output-multi-poly-trivial...
-- Added test: test-output-multi-polygon...
-- Added test: test-output-multi-tags...
-- Added test: test-output-pgsql-area...
-- Added test: test-output-pgsql-schema...
-- Added test: test-output-pgsql-tablespace...
-- Added test: test-output-pgsql-validgeom...
-- Added test: test-output-pgsql-z_order...
-- Added test: test-output-pgsql...
-- Added test: test-parse-diff...
-- Added test: test-parse-xml2...
-- Added test: test-persistent-node-cache...
-- Added test: test-pgsql-escape...
-- Added test: test-wildcard-match...
-- Found PythonInterp: /usr/bin/python (found version "2.7.5") 
-- Added test: regression-test-pbf (needs Python with psycopg2 module)
-- Configuring done
-- Generating done
-- Build files have been written to: /root/osm2pgsql-0.94.0/build

 ```

 - 编译

 ```
[root@demo build]# make && make install
Scanning dependencies of target osm2pgsql_lib
[  3%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/expire-tiles.cpp.o
[  6%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/geometry-processor.cpp.o
[  9%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/id-tracker.cpp.o
[ 12%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/middle-pgsql.cpp.o
[ 16%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/middle-ram.cpp.o
[ 19%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/middle.cpp.o
[ 22%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/node-persistent-cache.cpp.o
[ 25%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/node-ram-cache.cpp.o
[ 29%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/options.cpp.o
[ 32%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/osmdata.cpp.o
[ 35%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/osmium-builder.cpp.o
[ 38%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/output-gazetteer.cpp.o
[ 41%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/output-multi.cpp.o
[ 45%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/output-null.cpp.o
[ 48%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/output-pgsql.cpp.o
[ 51%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/output.cpp.o
[ 54%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/parse-osmium.cpp.o
[ 58%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/pgsql.cpp.o
[ 61%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/processor-line.cpp.o
[ 64%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/processor-point.cpp.o
[ 67%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/processor-polygon.cpp.o
[ 70%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/reprojection.cpp.o
[ 74%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/sprompt.cpp.o
[ 77%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/table.cpp.o
[ 80%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/taginfo.cpp.o
[ 83%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/tagtransform.cpp.o
[ 87%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/tagtransform-c.cpp.o
[ 90%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/util.cpp.o
[ 93%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/wildcmp.cpp.o
[ 96%] Building CXX object CMakeFiles/osm2pgsql_lib.dir/tagtransform-lua.cpp.o
Linking CXX static library libosm2pgsql.a
[ 96%] Built target osm2pgsql_lib
Scanning dependencies of target osm2pgsql
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

 - 查看

 ```
[root@demo build]# osm2pgsql -version
osm2pgsql version 0.94.0 (64 bit id space)

Osm2pgsql failed due to ERROR: Missing zoom level for tile expiry.
 ```

[3] 数据导入

 - 数据准备

 ```
openstreetmap 数据下载页面http://download.geofabrik.de/asia.html

[root@demo build]# cd /tmp
[root@demo tmp]# wget http://download.geofabrik.de/asia/china-latest.osm.pbf
[root@demo tmp]# chmod 755 china-latest.osm.pbf
 ```

 - 数据导入

```
[root@demo tmp]# su postgres
bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf 
osm2pgsql version 0.94.0 (64 bit id space)

Password:
Using built-in tag processing pipeline
Using projection SRS 3857 (Spherical Mercator)
Setting up table: planet_osm_point
Setting up table: planet_osm_line
Setting up table: planet_osm_polygon
Setting up table: planet_osm_roads
Allocating memory for dense node cache
Allocating dense node cache in one big chunk
Allocating memory for sparse node cache
Sharing dense sparse
Node-cache: cache=800MB, maxblocks=12800*65536, allocation method=11
Mid: pgsql, cache=800
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels

Reading in file: /tmp/china-latest.osm.pbf
Using PBF parser.
Processing: Node(55739k 224.8k/s) Way(4070k 26.60k/s) Relation(45850 487.77/s)  parse time: 495s
Node stats: total(55739733), max(5577202979) in 248s
Way stats: total(4070437), max(583356532) in 153s
Relation stats: total(45899), max(8244324) in 94s
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels
Using built-in tag processing pipeline
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels
Using built-in tag processing pipeline

Going over pending ways...
	1628899 ways are pending

Using 2 helper-processes
Finished processing 1628899 ways in 206 s

1628899 Pending ways took 206s at a rate of 7907.28/s
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads

Going over pending relations...
	0 relations are pending

Using 2 helper-processes
Finished processing 0 relations in 0 s

Committing transaction for planet_osm_point
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_line
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_polygon
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_roads
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_point
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_line
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_polygon
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_roads
WARNING:  there is no transaction in progress
Sorting data and creating indexes for planet_osm_line
Sorting data and creating indexes for planet_osm_point
Sorting data and creating indexes for planet_osm_roads
Sorting data and creating indexes for planet_osm_polygon
Copying planet_osm_point to cluster by geometry finished
Creating geometry index on planet_osm_point
Copying planet_osm_roads to cluster by geometry finished
Creating geometry index on planet_osm_roads
NOTICE:  Self-intersection at or near point 13317174.282055551 3548338.7415769785
Creating osm_id index on planet_osm_point
Creating indexes on planet_osm_point finished
All indexes on planet_osm_point created in 126s
Completed planet_osm_point
NOTICE:  Self-intersection at or near point 11985875.570417937 4115611.1799857486
Creating osm_id index on planet_osm_roads
Creating indexes on planet_osm_roads finished
All indexes on planet_osm_roads created in 140s
Completed planet_osm_roads
Copying planet_osm_line to cluster by geometry finished
Creating geometry index on planet_osm_line
Copying planet_osm_polygon to cluster by geometry finished
Creating geometry index on planet_osm_polygon
Creating osm_id index on planet_osm_line
Creating indexes on planet_osm_line finished
All indexes on planet_osm_line created in 328s
Completed planet_osm_line
Creating osm_id index on planet_osm_polygon
Creating indexes on planet_osm_polygon finished
All indexes on planet_osm_polygon created in 367s
Completed planet_osm_polygon
Stopping table: planet_osm_nodes
Stopped table: planet_osm_nodes in 0s
Stopping table: planet_osm_ways
Building index on table: planet_osm_ways
Stopped table: planet_osm_ways in 1075s
Stopping table: planet_osm_rels
Building index on table: planet_osm_rels
Stopped table: planet_osm_rels in 9s
node cache: stored: 52910936(94.92%), storage efficiency: 50.46% (dense blocks: 426, sparse nodes: 50683905), hit rate: 94.94%

Osm2pgsql took 2152s overall


bash-4.2$ exit
```

[4] openstreetmap-carto样式导入

 - 下载样式

```
[root@demo tmp]# wget -O openstreetmap-carto-master.zip https://codeload.github.com/gravitystorm/openstreetmap-carto/zip/master
--2018-04-26 19:10:47--  https://codeload.github.com/gravitystorm/openstreetmap-carto/zip/master
Resolving codeload.github.com (codeload.github.com)... 13.229.189.0, 13.250.162.133, 54.251.140.56
Connecting to codeload.github.com (codeload.github.com)|13.229.189.0|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2907152 (2.8M) [application/zip]
Saving to: ‘openstreetmap-carto-master.zip’

100%[=======================================>] 2,907,152    161KB/s   in 23s    

2018-04-26 19:11:12 (124 KB/s) - ‘openstreetmap-carto-master.zip’ saved [2907152/2907152]

```

 - 解压

 ```
[root@demo tmp]# unzip openstreetmap-carto-master.zip 

  inflating: openstreetmap-carto-master/water.mss  
finishing deferred symbolic links:
  openstreetmap-carto-master/scripts/lua/openstreetmap-carto.lua -> ../../openstreetmap-carto.lua

```

 - 移动

```
[root@demo tmp]# mv openstreetmap-carto-master /home/postgresql_data/

```

 - 导入

```
[root@demo tmp]# su postgres
bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf --style /home/postgresql_data/openstreetmap-carto-master/openstreetmap-carto.style
osm2pgsql version 0.94.0 (64 bit id space)

Password:
Using built-in tag processing pipeline
Using projection SRS 3857 (Spherical Mercator)
Setting up table: planet_osm_point
Setting up table: planet_osm_line
Setting up table: planet_osm_polygon
Setting up table: planet_osm_roads
Allocating memory for dense node cache
Allocating dense node cache in one big chunk
Allocating memory for sparse node cache
Sharing dense sparse
Node-cache: cache=800MB, maxblocks=12800*65536, allocation method=11
Mid: pgsql, cache=800
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels

Reading in file: /tmp/china-latest.osm.pbf
Using PBF parser.
Processing: Node(55739k 192.2k/s) Way(4070k 25.76k/s) Relation(45730 476.35/s)  parse time: 544s
Node stats: total(55739733), max(5577202979) in 290s
Way stats: total(4070437), max(583356532) in 158s
Relation stats: total(45899), max(8244324) in 96s
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels
Using built-in tag processing pipeline
Setting up table: planet_osm_nodes
Setting up table: planet_osm_ways
Setting up table: planet_osm_rels
Using built-in tag processing pipeline

Going over pending ways...
	1621055 ways are pending

Using 2 helper-processes
Finished processing 1621055 ways in 208 s

1621055 Pending ways took 208s at a rate of 7793.53/s
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads
Committing transaction for planet_osm_point
Committing transaction for planet_osm_line
Committing transaction for planet_osm_polygon
Committing transaction for planet_osm_roads

Going over pending relations...
	0 relations are pending

Using 2 helper-processes
Finished processing 0 relations in 0 s

Committing transaction for planet_osm_point
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_line
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_polygon
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_roads
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_point
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_line
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_polygon
WARNING:  there is no transaction in progress
Committing transaction for planet_osm_roads
WARNING:  there is no transaction in progress
Sorting data and creating indexes for planet_osm_line
Sorting data and creating indexes for planet_osm_point
Sorting data and creating indexes for planet_osm_polygon
Sorting data and creating indexes for planet_osm_roads
Copying planet_osm_point to cluster by geometry finished
Creating geometry index on planet_osm_point
Copying planet_osm_roads to cluster by geometry finished
Creating geometry index on planet_osm_roads
NOTICE:  Self-intersection at or near point 13317174.282055551 3548338.7415769785
Creating osm_id index on planet_osm_point
Creating indexes on planet_osm_point finished
All indexes on planet_osm_point created in 139s
Completed planet_osm_point
NOTICE:  Self-intersection at or near point 11985875.570417937 4115611.1799857486
Creating osm_id index on planet_osm_roads
Creating indexes on planet_osm_roads finished
All indexes on planet_osm_roads created in 148s
Completed planet_osm_roads
Copying planet_osm_line to cluster by geometry finished
Creating geometry index on planet_osm_line
Creating osm_id index on planet_osm_line
Creating indexes on planet_osm_line finished
All indexes on planet_osm_line created in 346s
Completed planet_osm_line
Copying planet_osm_polygon to cluster by geometry finished
Creating geometry index on planet_osm_polygon
Creating osm_id index on planet_osm_polygon
Creating indexes on planet_osm_polygon finished
All indexes on planet_osm_polygon created in 396s
Completed planet_osm_polygon
Stopping table: planet_osm_nodes
Stopped table: planet_osm_nodes in 0s
Stopping table: planet_osm_ways
Building index on table: planet_osm_ways
Stopped table: planet_osm_ways in 1189s
Stopping table: planet_osm_rels
Building index on table: planet_osm_rels
Stopped table: planet_osm_rels in 5s
node cache: stored: 52910936(94.92%), storage efficiency: 50.46% (dense blocks: 426, sparse nodes: 50683905), hit rate: 94.94%

Osm2pgsql took 2346s overall

```



