### 前提环境

> 使用上节中已经安装好的PostGis服务，详见[Centos7.2安装部署PostGis2.4.4](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2PostGis2.4.4.md)

> 使用上节配置好的PostgreSQL服务，详见[Centos7.2部署osm2pgsql-0.94.0](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E9%83%A8%E7%BD%B2osm2pgsql-0.94.0.md)


### 数据下载

[1] 中国地图陆图数据下载


[2] 中国地图海图数据下载








### 数据导入

[1] 陆图数据导入



[2] 海图数据导入







[3] 数据导入

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

> 数据导入完成