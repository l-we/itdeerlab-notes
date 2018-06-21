### 简要说明

> Tomcat在Linux下的安装配置 前提:已经有一台Linux操作系统，切使用root用户登录,有wget和vim命令
    
### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：study

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.200

### 系统检查

```
[root@study ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@study ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装

[1] 安装JDK 详见:[JDK在Centos7.2的安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/JDK/UserGuide/JDK%E5%9C%A8Centos7.2%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)

[2] Tomcat7下载

```
[root@study ~]# mkdir -p /opt/install
[root@study ~]# cd /opt/install
[root@study install]# wget http://192.168.0.220/source/tomcat/apache-tomcat-7.0.42.zip

注意： 我这里是自己搭建的源服务器，JDK的包从其他地方或者官网下载即可，或者本地上传也可以
```

[3] 解压

```
没有unzip命令时，使用以下命令进行安装
[root@study install]# yum install -y unzip
[root@study install]# unzip apache-tomcat-7.0.42.zip
[root@study install]# mv apache-tomcat-7.0.42 tomcat7
[root@study install]# rm -fr apache-tomcat-7.0.42.zip
```

[4] 配置

```
[root@study install]# cd tomcat7
[root@study tomcat7]# chmod +x bin/*

```

[5] 启动


```
[root@study tomcat7]# ./bin/startup.sh
```

### 结果验证

 - 服务验证

```
查看端口是否启动（默认端口）
若没有lsof命令，执行以下命令安装
[root@study tomcat7]# yum install lsof -y

[root@study tomcat7]# lsof -i :8080

COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    10718 root   45u  IPv6  30491      0t0  TCP *:webcache (LISTEN)

```

 - 浏览器验证

```
[root@study tomcat7]# systemctl stop firewalld
[root@study tomcat7]# systemctl disable firewalld
[root@study tomcat7]# systemctl status firewalld

浏览器地址栏中输入：http://IP:8080
```

![2018621141411](http://panrhkqz9.bkt.clouddn.com/2018621141411.png)

> 在页面看到三条腿的猫则配置完成，这样就可以部署一些JavaWeb的应用，建立自己的站点