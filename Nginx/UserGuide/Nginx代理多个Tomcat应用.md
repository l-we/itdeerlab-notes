### 同一台Linux机器Nginx代理多个Tomcat应用

> 一些企业的官网，访问量不是很大，但是只部署一个应用在一台服务器上，怎么感觉都是浪费。这时候一般都是一台服务器上部署多个应用，使用不同的域名进行访问。基于这个需求，给大家分享一下“一个Nginx服务器代理多个Tomcat（一个Tomcat单个应用）站点”的反向代理配置，实现节省服务器开支


### 所需要安装的软件

1. 安装JDK详见 [JDK在Linux下的安装配置](https://note.youdao.com/share/?id=7383d76f34f1c697ebedbf8a6450d969&type=notebook#/WEBe460749d3a6c81d4b4dd36c3151e3893)

2. 安装Tomcat详见 [Tomcat在Linux下的安装配置](https://note.youdao.com/share/?id=7383d76f34f1c697ebedbf8a6450d969&type=notebook#/DB4D73F018F54ACBB6FA6C2D36C09331)

3. 安装Nginx详见 [Nginx 在Linux下的安装配置](https://note.youdao.com/share/?id=7383d76f34f1c697ebedbf8a6450d969&type=notebook#/DB4D73F018F54ACBB6FA6C2D36C09331)

###  验证以上安装的软件已经是正常工作

1. Java

```
[root@study ~]# java -version

java version "1.8.0_152"
Java(TM) SE Runtime Environment (build 1.8.0_152-b16)
Java HotSpot(TM) 64-Bit Server VM (build 25.152-b16, mixed mode)
```

2. Tomcat(默认端口8080)

```
[root@study ~]# lsof -i :8080

COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    10718 root   45u  IPv6  30491      0t0  TCP *:webcache (LISTEN)
```

3. Nginx(默认端口80)

```
[root@study ~]# lsof -i :80

COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   17946  root    6u  IPv4  41378      0t0  TCP *:http (LISTEN)
nginx   17947 nginx    6u  IPv4  41378      0t0  TCP *:http (LISTEN)
```

### 以下我用三个网站项目工程做Demo

1.Demo1（单独部署时，访问根如：http://localhost:8080/ 出现页面显示“这是Demo1项目”）

2.Demo2（单独部署时，访问根如：http://localhost:8080/ 出现页面显示“这是Demo2项目”）

3.Demo3（单独部署时，访问根如：http://localhost:8080/ 出现页面显示“这是Demo3项目”）

### 以下我用三个域名来对应三个项目

1.www.demotest1.com对应Demo1项目

2.www.demotest2.com对应Demo2项目

3.www.demotest3.com对应Demo3项目

### 整体架构

![201862114954](http://panrhkqz9.bkt.clouddn.com/201862114954.png)

### 准备三个Tomcat的Web服务器

```
[root@study ~]# cd /opt/install

[root@study install]# cp -r tomcat7/ tomcat1
[root@study install]# cp -r tomcat7/ tomcat2
[root@study install]# cp -r tomcat7/ tomcat3
[root@study install]# ls
jdk1.8  tomcat1  tomcat2  tomcat3  tomcat7

停掉之前启动的tomcat7，并删除掉
[root@study install]# ./tomcat1/bin/shutdown.sh
Using CATALINA_BASE:   /opt/install/tomcat7
Using CATALINA_HOME:   /opt/install/tomcat7
Using CATALINA_TMPDIR: /opt/install/tomcat7/temp
Using JRE_HOME:        /opt/install/jdk1.8/jre
Using CLASSPATH:       /opt/install/tomcat7/bin/bootstrap.jar:/opt/install/tomcat7/bin/tomcat-juli.jar

[root@study install]# rm -fr tomcat7/
[root@study install]# ls
jdk1.8  tomcat1  tomcat2  tomcat3
```

> 查看默认配置

```
[root@study install]# cd tomcat1/conf/
[root@study conf]# vi server.xml

修改一下一些地方，使同一个Linux服务器中同时可以启动多个Tomcat

先把默认修改的地方列出来：

1.执行命令关闭的端口号，这个不能冲突
<Server port="8005" shutdown="SHUTDOWN">

2.对外提供Web访问的端口
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
              
3.AJP通讯端口
<!-- Define an AJP 1.3 Connector on port 8009 -->
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />

4.默认Servlet容器
<Engine name="Catalina" defaultHost="localhost">

5.虚拟主机
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

```

> 修改配置三个Tomcat配置

```
三个Demo我按照顺序进行修改
Demo1
    
    <Server port="8004" shutdown="SHUTDOWN">

    <Connector port="8070" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
              
    <Connector port="8008" protocol="AJP/1.3" redirectPort="8443" />

    <Engine name="Catalina" defaultHost="www.demotest1.com">

    <Host name="www.demotest1.com"  appBase="webapps"
                unpackWARs="true" autoDeploy="true">

Demo2
    
    <Server port="8005" shutdown="SHUTDOWN">

    <Connector port="8080" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
              
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />

    <Engine name="Catalina" defaultHost="www.demotest2.com">

    <Host name="www.demotest2.com"  appBase="webapps"
                unpackWARs="true" autoDeploy="true">
                
Demo3
    
    <Server port="8006" shutdown="SHUTDOWN">

    <Connector port="8090" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />
              
    <Connector port="8010" protocol="AJP/1.3" redirectPort="8443" />

    <Engine name="Catalina" defaultHost="www.demotest3.com">

    <Host name="www.demotest3.com"  appBase="webapps"
                unpackWARs="true" autoDeploy="true">

```

> 部署项目

```
rm -fr /opt/install/tomcat1/webapps/*
rm -fr /opt/install/tomcat2/webapps/*
rm -fr /opt/install/tomcat3/webapps/*

上传Demo1的项目ROOT.war到/opt/install/tomcat1/webapps/
上传Demo2的项目ROOT.war到/opt/install/tomcat2/webapps/
上传Demo3的项目ROOT.war到/opt/install/tomcat3/webapps/

```

> 启动三个Tomcat
这里的三个tomcat是复制过来的，所有bin目录下的命令都有权限执行

```
./opt/install/tomcat1/bin/start.sh
./opt/install/tomcat2/bin/start.sh
./opt/install/tomcat3/bin/start.sh
```

> 验证三个Tomcat能通过IP和端口正常访问

```
使用curl命令 若没有找到curl命令，使用[yum install curl -y] 进行安装

[root@study install]# curl http://localhost:8070

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Demo1</title>
</head>
<body>
	这是Demo1项目
</body>

[root@study install]# curl http://localhost:8080

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Demo2</title>
</head>
<body>
	这是Demo2项目
</body>
</html>

[root@study install]# curl http://localhost:8090

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Demo3</title>
</head>
<body>
	这是Demo3项目
</body>

```

![2018621141025](http://panrhkqz9.bkt.clouddn.com/2018621141025.png)

### 修改Nginx

> 添加配置

 - 编辑主配置文件

```
vi /etc/nginx/nginx.conf

....

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

这一行"include /etc/nginx/conf.d/*.conf;" 如果是注释起来的，打开即可。我使用的版本默认是打开的
```
 - 创建配置文件
 
```
cd /conf.d
vi demotest.conf

upstream tomcat_server1 {
    server localhost:8070;
}

upstream tomcat_server2 {
    server localhost:8080;
}

upstream tomcat_server3 {
    server localhost:8090;
}

server {
    listen       80;
    server_name  www.demotest1.com;
    location / {
        proxy_pass http://tomcat_server1;
        index index.jsp index.html index.htm;
    }
}

server {
    listen       80;
    server_name  www.demotest2.com;
    location / {
        proxy_pass http://tomcat_server2;
        index index.jsp index.html index.htm;
    }
}

server {
    listen       80;
    server_name  www.demotest3.com;
    location / {
        proxy_pass http://tomcat_server3;
        index index.jsp index.html index.htm;
    }
}

```


> 重启Nginx

```
systemctl restart nginx
lsof -i :80
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   18083  root    6u  IPv4  43040      0t0  TCP *:http (LISTEN)
nginx   18084 nginx    6u  IPv4  43040      0t0  TCP *:http (LISTEN)
```


> 修改服务器本地映射

```
vi /etc/hosts

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.1.200 www.demotest1.com
192.168.1.200 www.demotest2.com
192.168.1.200 www.demotest3.com
```

> 修改window本地映射

为什么要配置这里呢？
因为这里的域名是不是真正的经过DNS服务器转发的域名，所以通过配置hosts文件进行本地装发来模拟域名访问

```
进入【C盘】--> 【Windows】 --> 【System32】 --> 【drivers】 --> 【etc】下

修改hosts文件，添加如下信息
192.168.1.200   www.demotest1.com
192.168.1.200   www.demotest2.com
192.168.1.200   www.demotest3.com
```

> 验证

```

浏览器中访问：

http://www.demotest1.com/
http://www.demotest2.com/
http://www.demotest3.com/
```

出现:
![2018621141054](http://panrhkqz9.bkt.clouddn.com/2018621141054.png)

> 原因

```
1.防火墙不让访问

2.Selinux不让通过
```

> 解决

```
第一可能问题：
[root@study install]# systemctl stop firewalld
[root@study install]# systemctl disable firewalld
[root@study install]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)

Dec 26 14:21:05 study systemd[1]: Starting firewalld - dynamic firewall daemon...
Dec 26 14:21:07 study systemd[1]: Started firewalld - dynamic firewall daemon.
Dec 26 14:47:06 study systemd[1]: Stopping firewalld - dynamic firewall daemon...
Dec 26 14:47:07 study systemd[1]: Stopped firewalld - dynamic firewall daemon.
Dec 26 16:53:08 study systemd[1]: Stopped firewalld - dynamic firewall daemon.
Dec 26 19:00:28 study systemd[1]: Starting firewalld - dynamic firewall daemon...
Dec 26 19:00:29 study systemd[1]: Started firewalld - dynamic firewall daemon.
Dec 26 19:00:47 study systemd[1]: Stopping firewalld - dynamic firewall daemon...
Dec 26 19:00:49 study systemd[1]: Stopped firewalld - dynamic firewall daemon.

访问还是出现这个问题

第二可能问题：

/usr/sbin/setsebool httpd_can_network_connect true
或者
setenforce 0
或者
[root@study install]# vi /etc/selinux/config
SELINUX=disabled

验证成功
```

> 结果

![2018621141113](http://panrhkqz9.bkt.clouddn.com/2018621141113.png)

> 本案例讲解的是使用同一台服务器，模拟一个Nginx代理三个tomcat的应用，通过上面顺序步骤的配置，即可实现此功能
之后还会有一个Nginx代理一个Tomcat的多个应用及一个应用的负载配置，敬请关注

