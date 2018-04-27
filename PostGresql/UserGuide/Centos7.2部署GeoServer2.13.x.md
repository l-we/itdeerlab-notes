### 环境准备

 - CentOS7.2.1511操作系统
 - 系统语言为英文
 - 系统最小化安装
 - Linux内核版本3.10.0
 - 使用的用户：root
 - 登录密码为：123456
 - 主机名称为：demo

### 系统检查

```
[root@demo ~]# uname -a
Linux demo 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

[root@demo ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```

### 软件安装

[1] 安装JDK

> 安装JDK详见:[JDK在Centos7.2的安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/JDK/UserGuide/JDK%E5%9C%A8Centos7.2%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)

[2] 安装Tomcat

> 安装Tomcat详见:[Tomcat7在Centos7.2的安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tomcat/UserGuide/Tomcat7%E5%9C%A8Centos7.2%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)

[3] 安装GeoServer

 - 下载

```
[root@demo ~]# cd /opt/install/
[root@demo install]# wget https://build.geoserver.org/geoserver/2.13.x/geoserver-2.13.x-2018-04-24-war.zip

......
Length: 82690104 (79M) [application/zip]
Saving to: ‘geoserver-2.13.x-2018-04-24-war.zip’

100%[======================================>] 82,690,104   966KB/s   in 18m 32s

2018-04-26 19:58:37 (72.6 KB/s) - ‘geoserver-2.13.x-2018-04-24-war.zip’ saved [82690104/82690104]
```

 - 解压

```
[root@demo install]# mkdir geoserver && unzip geoserver-2.13.x-2018-04-24-war.zip -d geoserver && rm -fr geoserver-2.13.x-2018-04-24-war.zip

Archive:  geoserver-2.13.x-2018-04-24-war.zip
  inflating: geoserver/geoserver.war  
  inflating: geoserver/GPL.txt       
  inflating: geoserver/LICENSE.txt   
   creating: geoserver/target/
  inflating: geoserver/target/VERSION.txt 
```

 - 放入WEB服务器

```
[root@demo install]# cp geoserver/geoserver.war tomcat7/webapps/

[root@demo install]# cd tomcat7/

[root@demo tomcat7]# ./bin/shutdown.sh

[root@demo tomcat7]# ./bin/startup.sh
```

### 结果验证

[1] 浏览器访问

 - 打开 http://IP:8080/geoserver

 - 用户名为:admin

 - 密码为: geoserver

> 至此GeoServer运行成功