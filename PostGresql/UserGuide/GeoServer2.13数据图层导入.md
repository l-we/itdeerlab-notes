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
[root@demo tmp]# su - postgres
-bash-4.2$ cd /tmp

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

-bash-4.2$ exit
```

### 操作GeoServer

[1] 创建工作空间

> 点击 [工作区] --> [添加新的工作区] --> [Name]填值 --> [命名空间URI]填值 --> 勾选[默认工作区] --> 提交

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-8.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-9.png)

> 点击[工作区名称] chinaosm 点击进去 --> 勾选[服务] 下所有的服务 --> 启用按钮

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-10.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-11.png)

[2] 新建矢量数据源

> 点击 [数据存储] --> [添加新的数据存储] --> 点击[s矢量数据源] -->点击 [PostGis] -->填写 存储的基本信息

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-12.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-13.png)

> 工作区 - 数据源名称 - 说明 - 连接参数 - host - port - database - scheam - user - passwd 

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-14.png)

[3] 创建样式和图表

> 自动创建

```
[root@demo tmp]# cd osmsld/
[root@demo osmsld]# unzip sld.zip 
Archive:  sld.zip
   creating: sld/
  inflating: sld/aero-poly.sld       
  inflating: sld/agriculture.sld     
  ...... 
  inflating: sld/water.sld 
```

```

[root@demo osmsld]# chmod +x /tmp/osmsld/sld/*

[root@demo osmsld]# vim /tmp/osmsld/SLD_create.sh


#  script to add layer/style information
#  for every SLD file in our collection
#
restapi=http://localhost:8080/geoserver/rest
login=admin:geoserver
workspace=chinaosm
store=chinaosmds

for sldfile in *.sld; do

  # strip the extension from the filename to use for layer/style names
  layername=`basename $sldfile .sld`

  # create a new featuretype in the store, assuming the table
  # already exists in the database and is named $layername
  # this step automatically creates a layer of the same name
  # as a side effect
  curl -v -u $login -XPOST -H "Content-type: text/xml" \
    -d "<featureType><name>$layername</name></featureType>" \
    $restapi/workspaces/$workspace/datastores/$store/featuretypes?recalculate=nativebbox,latlonbbox

  # create an empty style object in the workspace, using the same name
  curl -v -u $login -XPOST -H "Content-type: text/xml" \
    -d "<style><name>$layername</name><filename>$sldfile</filename></style>" \
    $restapi/workspaces/$workspace/styles

  # upload the SLD definition to the style
  curl -v -u $login -XPUT -H "Content-type: application/vnd.ogc.sld+xml" \
    -d @$sldfile \
    $restapi/workspaces/$workspace/styles/$layername

  # associate the style with the layer as the default style
  curl -v -u $login -XPUT -H "Content-type: text/xml" \
    -d "<layer><enabled>true</enabled><defaultStyle><name>$layername</name><workspace>$workspace</workspace></defaultStyle></layer>" \
    $restapi/layers/$workspace:$layername

done


#curl -v -u admin:@pp\$4boundleSS -XPOST -d@layergroup.xml -H "Content-type: text/xml" \
#  http://apps.opengeo.org/geoserver/rest/workspaces/osm/layergroups


```

```
[root@demo osmsld]# cd sld

[root@demo sld]# sh /tmp/osmsld/sld/SLD_create.sh 

* Connected to 192.168.0.119 (192.168.0.119) port 8080 (#0)
* Server auth using Basic with user 'admin'
> PUT /geoserver/rest/layers/chinaosm:water HTTP/1.1
> Authorization: Basic YWRtaW46Z2Vvc2VydmVy
> User-Agent: curl/7.29.0
> Host: 192.168.0.119:8080
> Accept: */*
> Content-type: text/xml
> Content-Length: 116
> 
* upload completely sent off: 116 out of 116 bytes
< HTTP/1.1 200 OK
< Server: Apache-Coyote/1.1
< X-Frame-Options: SAMEORIGIN
< Content-Length: 0
< Date: Sat, 28 Apr 2018 23:05:05 GMT
< 
* Connection #0 to host 192.168.0.119 left intact


[root@demo sld]# cd ..
```

[4] 查看图层及样式

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-15.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.28-16.png)


[5] 创建图层组

> 编辑layergroup.xml

```
[root@demo osmsld]# vim layergroup.xml

<layerGroup>
  <name>osm</name>
  <title>OpenStreetMap Base</title>
  <layers>
    <layer>agriculture</layer>
    <layer>beach</layer>
    <layer>forest</layer>
    <layer>grass</layer>
    <layer>water-outline</layer>
    <layer>water</layer>
    <layer>ocean</layer>
    <layer>park</layer>
    <layer>aero-poly</layer>
    <layer>parking-area</layer>
    <layer>amenity-areas</layer>
    <layer>building</layer>
    <layer>route-tunnels</layer>
    <layer>route-bridge-0</layer>
    <layer>route-bridge-1</layer>
    <layer>route-bridge-2</layer>
    <layer>route-bridge-3</layer>
    <layer>route-bridge-4</layer>
    <layer>route-bridge-5</layer>
    <layer>route-line</layer>
    <layer>route-fill</layer>
    <layer>placenames-medium</layer>
    <layer>highway-label</layer>
  </layers>
</layerGroup>

删除<layer>ocean</layer>这一行
```

> 创建图层组

```
[root@demo osmsld]# curl -v -u admin:geoserver -XPOST -d@layergroup.xml -H "Content-type: text/xml" http://localhost:8080/geoserver/rest/workspaces/chinaosm/layergroups
* About to connect() to localhost port 8080 (#0)
*   Trying ::1...
* Connected to localhost (::1) port 8080 (#0)
* Server auth using Basic with user 'admin'
> POST /geoserver/rest/workspaces/chinaosm/layergroups HTTP/1.1
> Authorization: Basic YWRtaW46Z2Vvc2VydmVy
> User-Agent: curl/7.29.0
> Host: localhost:8080
> Accept: */*
> Content-type: text/xml
> Content-Length: 755
> 
* upload completely sent off: 755 out of 755 bytes
< HTTP/1.1 201 Created
< Server: Apache-Coyote/1.1
< X-Frame-Options: SAMEORIGIN
< Location: http://localhost:8080/geoserver/rest/layergroups/osm
< Content-Type: application/json;charset=ISO-8859-1
< Content-Length: 3
< Date: Sat, 28 Apr 2018 23:21:49 GMT
< 
* Connection #0 to host localhost left intact
```

> 查看图层组

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-1.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-2.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-3.png)


[6] 添加中文字体

 - 安装字体管理工具,详见[Centos7.2添加微软字体]()

 - 查看字体情况

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-4.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-5.png)

[7] 设置样式

> 更改样式中的[DejaVu Sans] 替换成 [Microsoft YaHei] 然后点击[Validate] 之后点击[Apply] 之后 [提交]

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-6.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-7.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/PostGresql/2018.04.30-8.png)


[8] 最终结果


