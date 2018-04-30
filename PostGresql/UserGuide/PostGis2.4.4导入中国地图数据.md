### 前提环境

> 使用上节中已经安装好的PostGis服务，详见[Centos7.2安装部署PostGis2.4.4](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2PostGis2.4.4.md)

> 使用上节配置好的PostgreSQL服务，详见[Centos7.2部署osm2pgsql-0.94.0](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E9%83%A8%E7%BD%B2osm2pgsql-0.94.0.md)


### 数据下载

[1] 中国地图陆图数据下载

> 下载页面地址: http://download.geofabrik.de/asia.html

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-5.png)

```
[root@demo ~]# wget http://download.geofabrik.de/asia/china-latest.osm.pbf

Connecting to download.geofabrik.de (download.geofabrik.de)|144.76.80.19|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 440332372 (420M) [application/octet-stream]
Saving to: ‘china-latest.osm.pbf’
......
```

[2] 中国地图海图数据下载

> 下载页面地址: http://openstreetmapdata.com/data/water-polygons

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-6.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-7.png)

```
[root@demo ~]# wget http://data.openstreetmapdata.com/water-polygons-split-3857.zip

Connecting to data.openstreetmapdata.com (data.openstreetmapdata.com)|144.76.68.74|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 530117251 (506M) [application/zip]
Saving to: ‘water-polygons-split-3857.zip’
......
```

[3] openstreemap的carto样式

> 下载页面地址: https://github.com/gravitystorm/openstreetmap-carto

```
[root@demo ~]# wget -O openstreetmap-carto-master.zip https://codeload.github.com/gravitystorm/openstreetmap-carto/zip/master

......
100%[=======================================>] 2,907,152    161KB/s   in 23s    

2018-04-26 19:11:12 (124 KB/s) - ‘openstreetmap-carto-master.zip’ saved [2907152/2907152]
```

### 数据整理

[1] 移动数据到tmp目录

```
[root@demo ~]# mv china-latest.osm.pbf water-polygons-split-3857.zip openstreetmap-carto-master.zip /tmp/

[root@demo ~]# mv cd /tmp/

[root@demo tmp]# 
```

[2] 对数据更改权限(防止权限问题提前更改)

```
[root@demo tmp]# chmod 755 ./*

[root@demo tmp]# ll

total 946880
-rwxr-xr-x. 1 root root 439481072 Apr 28 19:37 china-latest.osm.pbf
-rwxr-xr-x. 1 root root   2907051 Apr 28 20:09 openstreetmap-carto-master.zip
-rwxr-xr-x. 1 root root 530117251 Apr 28 19:36 water-polygons-split-3857.zip
```

[3] 解压数据

 - 解压海图压缩包

```
[root@demo tmp]# unzip water-polygons-split-3857.zip 

Archive:  water-polygons-split-3857.zip
  inflating: water-polygons-split-3857/README  
 extracting: water-polygons-split-3857/water_polygons.cpg  
  inflating: water-polygons-split-3857/water_polygons.dbf  
  inflating: water-polygons-split-3857/water_polygons.prj  
  inflating: water-polygons-split-3857/water_polygons.shp  
  inflating: water-polygons-split-3857/water_polygons.shx 
```

 - 解压样式压缩包

```
[root@demo tmp]# unzip openstreetmap-carto-master.zip

  inflating: openstreetmap-carto-master/water.mss  
finishing deferred symbolic links:
  openstreetmap-carto-master/scripts/lua/openstreetmap-carto.lua -> ../../openstreetmap-carto.lua

```

[4] 样式数据设置(移动到PostgreSQL的数据目录)

```
[root@demo tmp]# mv openstreetmap-carto-master /home/postgresql_data/
```

### 数据导入

[1] 陆图数据导入

> 只导入地图

```
[root@demo tmp]# su postgres

bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf

osm2pgsql version 0.94.0 (64 bit id space)

Password: 12345678
Using built-in tag processing pipeline
......
Sorting data and creating indexes for planet_osm_point
Sorting data and creating indexes for planet_osm_line
Sorting data and creating indexes for planet_osm_polygon
Sorting data and creating indexes for planet_osm_roads
Copying planet_osm_point to cluster by geometry finished
Creating geometry index on planet_osm_point
NOTICE:  Self-intersection at or near point 13317174.282055551 3548338.7415769785
Copying planet_osm_roads to cluster by geometry finished
Creating geometry index on planet_osm_roads
NOTICE:  Self-intersection at or near point 11985875.570417937 4115611.1799857486
Creating osm_id index on planet_osm_point
Creating indexes on planet_osm_point finished
All indexes on planet_osm_point created in 151s
Completed planet_osm_point
Creating osm_id index on planet_osm_roads
Creating indexes on planet_osm_roads finished
All indexes on planet_osm_roads created in 158s
Completed planet_osm_roads
Copying planet_osm_line to cluster by geometry finished
Creating geometry index on planet_osm_line
Copying planet_osm_polygon to cluster by geometry finished
Creating geometry index on planet_osm_polygon
Creating osm_id index on planet_osm_polygon
Creating indexes on planet_osm_polygon finished
All indexes on planet_osm_polygon created in 245s
Completed planet_osm_polygon
Creating osm_id index on planet_osm_line
Creating indexes on planet_osm_line finished
All indexes on planet_osm_line created in 282s
Completed planet_osm_line
Stopping table: planet_osm_nodes
Stopped table: planet_osm_nodes in 0s
Stopping table: planet_osm_ways
Building index on table: planet_osm_ways
Stopped table: planet_osm_ways in 1396s
Stopping table: planet_osm_rels
Building index on table: planet_osm_rels
Stopped table: planet_osm_rels in 15s
node cache: stored: 52911860(94.66%), storage efficiency: 50.46% (dense blocks: 426, sparse nodes: 50683905), hit rate: 94.66%

Osm2pgsql took 2285s overall
```

> 导入样式及地图

```
[root@demo tmp]# su postgres

bash-4.2$ osm2pgsql -s -U demo -H 127.0.0.1 -P 5432 -W -d demogisdb /tmp/china-latest.osm.pbf --style /home/postgresql_data/openstreetmap-carto-master/openstreetmap-carto.style

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

中间的时间需要一会,请耐心等待.....
```

[2] 海图数据导入

```
shp2pgsql -s 3857 -I -D /tmp/water-polygons-split-3857/water_polygons.shp ocean_all | psql -d demogisdb -U demo

bash-4.2$ exit
[root@demo tmp]# cd
[root@demo ~]# 
```




> 至此地图数据导入完成