
### 应用介绍

> 这是一个整体的应用，笔记记录呢，不可能整体的一下全部记下来，我这里分模块记录。整体的需求是这样的。

> 准备使用资源卫星（landsta8）的图片数据，实时更新数据到HDFS文件系统中，然后对图片数据进行栅格化，栅格化后的数据还保留在HDFS文件系统中，同时能导入到PostGis这样的空间数据库中，然后把OpenStreetMap的地图原图入库，通过GeoServer把栅格图和地图贴合做可视化。

> 我这边把整体简单化了一下做一个整体的Demo，所以是这样的，下载OpenStreetMap的一部分数据，导入到空间数据库中，先显示出地图来。然后下载landsta8的图片数据进行栅格化，然后入库，最后把栅格化数据和地图贴合展现，最后整体过程的数据存放早HDFS文件系统。


### 第一步把地图原图可视化

[1] 选择软件及平台

```
选择的平台为CRH大数据基础平台，软件使用PostgreSQL、PostGis、GeoServer。平台和操作系统已经安装完成，这里就不赘述了。
```

[2] 实施过程

 1. 安装PostgreSQL，详见[Centos7.2安装PostGreSQL9.5](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85PostGreSQL9.5.md)

 2. 简单配置PosgreSQL，详见[PostGreSQL9.5的简单配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/PostGreSQL9.5%E7%9A%84%E7%AE%80%E5%8D%95%E9%85%8D%E7%BD%AE.md)

 3. 在Window上安装一个客户端，详见[PostgreSQL9.5的pgAdmin客户端安装使用](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/PostgreSQL9.5%E7%9A%84pgAdmin%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%AE%89%E8%A3%85%E4%BD%BF%E7%94%A8.md)

 4. 安装PostGis，详见[Centos7.2安装部署PostGis2.4.4](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2PostGis2.4.4.md)

 5. 部署osm2pgsql，详见[Centos7.2安装部署PostGis2.4.4](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E9%83%A8%E7%BD%B2osm2pgsql-0.94.0.md)