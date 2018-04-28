### 前提环境

> 使用上节中已经安装好的GeoServer服务，详见[Centos7.2部署GeoServer2.13.x](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/Centos7.2%E9%83%A8%E7%BD%B2GeoServer2.13.x.md)


### 更改Tomcat的JVM设置

```
[root@demo tomcat7]# vim bin/catalina.sh

添加下面一句
# OS specific support.  $var _must_ be set to either true or false.
JAVA_OPTS="-Xms1024m -Xmx4096m -Xss1024K -XX:PermSize=512m -XX:MaxPermSize=2048m"
```

### 重启服务

```
[root@demo tomcat7]# ./bin/shutdown.sh

[root@demo tomcat7]# ./bin/start.sh
```