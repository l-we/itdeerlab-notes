### 简要说明

> Kafka-Manager是雅虎公司的工程师们，为了简化工程师和运维人员维护Kafka集群的工作而开发的一款管理软件，后来开源出来，今天给大家介绍一下这款软件的源码编译。

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：work

 > 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.210

### 系统检查

```
[root@work ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
[root@work ~]# uname -a
Linux work 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 编译环境准备

[1] JDK安装

> 详情请参考 [JDK在Centos7.2的安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/JDK/UserGuide/JDK%E5%9C%A8Centos7.2%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)


[2] 安装sbt

```
[root@work ~]# mkdir -p /opt/install
[root@work ~]# cd /opt/install/
```

 - 下载(官网下载地址：https://www.scala-sbt.org/download.html)

```
[root@work install]# wget https://piccolo.link/sbt-1.1.4.zip
--2018-05-23 08:32:09--  https://piccolo.link/sbt-1.1.4.zip
Saving to: ‘sbt-1.1.4.zip’

100%[===================================>] 45,989,766  1.69MB/s   in 82s    

2018-05-23 08:33:38 (545 KB/s) - ‘sbt-1.1.4.zip’ saved [45989766/45989766]
```

 - 解压

```
[root@work install]# ls
sbt-1.1.4.zip
[root@work install]# unzip sbt-1.1.4.zip 
Archive:  sbt-1.1.4.zip
  inflating: sbt/conf/sbtconfig.txt  
  inflating: sbt/conf/sbtopts         
  inflating: sbt/lib/local-preloaded/com.trueaccord.scalapb/scalapb-runtime_2.12/0.6.0/jars/scalapb-runtime_2.12.jar.md5  
[root@work install]# ls
sbt  sbt-1.1.4.zip
[root@work install]# rm -fr sbt-1.1.4.zip
```

 - 配置

```
[root@work install]# ls sbt
bin  conf  lib
[root@work install]# vim /etc/profile.d/sbt.sh

export SBT_HOME=/opt/install/sbt
export PATH=$SBT_HOME/bin:$PATH

[root@work install]# source /etc/profile
```

 - 验证

```
[root@work install]# sbt sbtVersion
[info] Loading project definition from /root/project
[info] Set current project to root (in build file:/root/)
[info] 1.1.4

```

### Kafka-Manager源码准备

 - 下载地址（选择版本 https://github.com/yahoo/kafka-manager/releases）

```
[root@work install]# wget https://github.com/yahoo/kafka-manager/archive/1.3.3.16.tar.gz
Location: https://codeload.github.com/yahoo/kafka-manager/tar.gz/1.3.3.16 [following]
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/x-gzip]
Saving to: ‘1.3.3.16.tar.gz’

    [               <=>                  ] 953,657      216KB/s   in 4.3s   

2018-05-23 22:26:54 (216 KB/s) - ‘1.3.3.16.tar.gz’ saved [953657]

```

 - 解压

```
[root@work install]# tar -zxf 1.3.3.16.tar.gz
[root@work install]# ls
1.3.3.16.tar.gz  kafka-manager-1.3.3.16  sbt
[root@work install]# cd kafka-manager-1.3.3.16/
```

### 编译

```
[root@work kafka-manager-1.3.3.16]# sbt clean dist
Getting org.scala-sbt sbt 0.13.9  (this may take some time)...
......

经过很久很久的时间之后，我终于熬不住了，这里时是夜里太晚了，没来得及看到结果，早上重新跑了一次。

...
[info] Done packaging.
[info] 
[info] Your package is ready in /opt/install/kafka-manager-1.3.3.16/target/universal/kafka-manager-1.3.3.16.zip
[info] 
[success] Total time: 69 s, completed May 24, 2018 6:56:43 AM
```

> 编译完以后，生成的包会在kafka-manager-1.3.3.16/target/universal 下面。生成的包只需要java环境就可以运行了，在部署的机器上不需要安装sbt。

> 看一眼成果

```
[root@work kafka-manager-1.3.3.16]# ll target/universal/
total 59280
-rw-r--r-- 1 root root 60701909 May 24 06:56 kafka-manager-1.3.3.16.zip
drwxr-xr-x 3 root root       16 May 24 06:56 tmp
```

> 至此Kafka-Manager就编译完成了，下一步开始做测试，验证打出来的包是可以使用的。