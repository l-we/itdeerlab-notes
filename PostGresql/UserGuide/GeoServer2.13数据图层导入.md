### 前提环境

> 简单配置GeoServer2.13, 详见[GeoServer2.13的简单配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/GeoServer2.13%E7%9A%84%E7%AE%80%E5%8D%95%E9%85%8D%E7%BD%AE.md)


### 创建图层表

[1] 下载自动创建程序

```
[root@demo tmp]# wget http://files.cnblogs.com/files/think8848/osmsld.zip

--2018-04-28 23:38:02--  http://files.cnblogs.com/files/think8848/osmsld.zip
Resolving files.cnblogs.com (files.cnblogs.com)... 120.55.28.79, 120.55.29.38
Connecting to files.cnblogs.com (files.cnblogs.com)|120.55.28.79|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 52527 (51K) [application/x-zip-compressed]
Saving to: ‘osmsld.zip’
.......
```

[2] 解压

```
[root@demo tmp]# unzip osmsld.zip 

Archive:  osmsld.zip
   creating: osmsld/
  inflating: osmsld/SLD_create.sh    
  inflating: osmsld/SLD_delete.sh    
  inflating: osmsld/create_tables.sql  
  inflating: osmsld/create_views.sql  
  inflating: osmsld/drop_tables.sql  
  inflating: osmsld/drop_views.sql   
  inflating: osmsld/layergroup.xml   
  inflating: osmsld/sld.zip 
```

[3] 脚本授权

```
[root@demo tmp]# chmod +x osmsld/*

[root@demo tmp]# ll osmsld
total 172
-rwxr-xr-x. 1 root root   9838 Oct 30  2016 create_tables.sql
-rwxr-xr-x. 1 root root   7394 Oct 30  2016 create_views.sql
-rwxr-xr-x. 1 root root    866 Oct 30  2016 drop_tables.sql
-rwxr-xr-x. 1 root root    842 Oct 30  2016 drop_views.sql
-rwxr-xr-x. 1 root root    807 Oct 30  2016 layergroup.xml
-rwxr-xr-x. 1 root root   1641 Oct 30  2016 SLD_create.sh
-rwxr-xr-x. 1 root root    758 Oct 30  2016 SLD_delete.sh
-rwxr-xr-x. 1 root root 134101 Oct 30  2016 sld.zip
```

[4] 创建表

```
bash-4.2$ psql -U postgres -W -d demogisdb -a -f /tmp/osmsld/create_tables.sql
Password for user postgres: 12345678
DROP TABLE IF EXISTS "aero-poly";
psql:/tmp/osmsld/create_tables.sql:1: NOTICE:  table "aero-poly" does not exist, skipping
DROP TABLE
CREATE TABLE "aero-poly" AS ( 
  SELECT way,aeroway 
  FROM planet_osm_polygon
  WHERE "aeroway" IN ('apron','runway','taxiway','helipad')
  ORDER BY z_order ASC );
......
SELECT 1815
CREATE INDEX "wetland_way_idx" ON "wetland" USING gist (way);
CREATE INDEX

bash-4.2$ exit
```

### 操作GeoServer

[1] 创建工作空间

> 点击 [工作区] --> [添加新的工作区] --> [Name]填值 --> [命名空间URI]填值 --> 勾选[默认工作区] --> 提交

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-8.png

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-9.png

> 点击[工作区名称] chinaosm 点击进去 --> 勾选[服务] 下所有的服务 --> 启用按钮

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-10.png

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-11.png

[2] 新建矢量数据源

> 点击 [数据存储] --> [添加新的数据存储] --> 点击[s矢量数据源] -->点击 [PostGis] -->填写 存储的基本信息

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-12.png

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-13.png

> 工作区 - 数据源名称 - 说明 - 连接参数 - host - port - database - scheam - user - passwd 

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-14.png

[3] 创建样式和图表

> 自动创建

```
[root@demo tmp]# cd osmsld/
[root@demo osmsld]# unzip sld.zip 
Archive:  sld.zip
   creating: sld/
  inflating: sld/aero-poly.sld       
  inflating: sld/agriculture.sld     
  inflating: sld/amenity-areas.sld   
  inflating: sld/beach.sld           
  inflating: sld/building.sld        
  inflating: sld/forest.sld          
  inflating: sld/grass.sld           
  inflating: sld/highway-label.sld   
  inflating: sld/ocean.sld           
  inflating: sld/park.sld            
  inflating: sld/parking-area.sld    
  inflating: sld/placenames-medium.sld  
  inflating: sld/route-bridge-0.sld  
  inflating: sld/route-bridge-1.sld  
  inflating: sld/route-bridge-2.sld  
  inflating: sld/route-bridge-3.sld  
  inflating: sld/route-bridge-4.sld  
  inflating: sld/route-bridge-5.sld  
  inflating: sld/route-fill.sld      
  inflating: sld/route-line.sld      
  inflating: sld/route-tunnels.sld   
  inflating: sld/route-turning-circles.sld  
  inflating: sld/SLD_create.sh       
  inflating: sld/SLD_delete.sh       
  inflating: sld/water-outline.sld   
  inflating: sld/water.sld 
```

```

```

