
### Linux 环境下安装Maven

> 需要提前安装好JDK，安装JDK可以参考 [JDK在Centos7.2的安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/JDK/UserGuide/JDK%E5%9C%A8Centos7.2%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)

```
[root@itdeer opt]# java -version
java version "1.8.0_112"
Java(TM) SE Runtime Environment (build 1.8.0_112-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b15, mixed mode)
```

> Maven的介绍这里就不赘述了，可以看一下 [Window下安装Maven](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Maven/UserGuide/Window%E4%B8%8B%E5%AE%89%E8%A3%85Maven.md)

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：itdeer

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.200

### 系统检查

```
[root@itdeer ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@itdeer ~]# uname -a
Linux itdeer 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装


[1] 安装包下载

> 下载地址：https://maven.apache.org/download.cgi

 - 找到下载页面

 ![201862118121](http://note.itdeer.cn/201862118121.png)

```
[root@itdeer ~]# cd /opt/

[root@itdeer opt]# wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.tar.gz
--2018-06-21 21:52:21--  http://mirror.bit.edu.cn/apache/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.tar.gz
Resolving mirror.bit.edu.cn (mirror.bit.edu.cn)... 202.204.80.77, 2001:da8:204:2001:250:56ff:fea1:22
Connecting to mirror.bit.edu.cn (mirror.bit.edu.cn)|202.204.80.77|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 8799579 (8.4M) [application/octet-stream]
Saving to: ‘apache-maven-3.5.3-bin.tar.gz’

100%[===============================================>] 8,799,579    401KB/s   in 39s    

2018-06-21 21:53:00 (221 KB/s) - ‘apache-maven-3.5.3-bin.tar.gz’ saved [8799579/8799579]

```

> 这里下载3.5.3的版本

[2] 解压改名称

```
[root@itdeer opt]# tar -zxf apache-maven-3.5.3-bin.tar.gz

[root@itdeer opt]# mv apache-maven-3.5.3 maven

[root@itdeer opt]# rm -fr apache-maven-3.5.3-bin.tar.gz
```

[3] 配置

```
[root@itdeer opt]# vim /etc/profile.d/maven.sh

export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH

[root@itdeer opt]# source /etc/profile
```

### 验证

```
[root@itdeer opt]# mvn -v
Apache Maven 3.5.3 (3383c37e1f9e9b3bc3df5050c29c8aff9f295297; 2018-02-25T03:49:05+08:00)
Maven home: /opt/maven
Java version: 1.8.0_112, vendor: Oracle Corporation
Java home: /opt/jdk/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-327.el7.x86_64", arch: "amd64", family: "unix"
```

> Maven安装成功，接下来可以使用Maven管理自己的项目了。