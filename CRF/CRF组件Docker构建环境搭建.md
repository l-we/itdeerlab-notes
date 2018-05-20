
> HDF工作流平台组件的编译环境,这里使用的是Docker镜像的方式,这里制作的Docker镜像,首先安装Docker

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：study

 > 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.228

### 系统检查

```
[root@study ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@study ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 安装Docker运行容器

[1] 安装Docker详见 [Docker在Centos7.2上的安装启动](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Docker/UserGuide/Docker%E5%9C%A8Centos7.2%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%90%AF%E5%8A%A8.md)

[2] 简单配置Docker详见 [Docker默认存储和加速配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Docker/UserGuide/Docker%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E5%92%8C%E5%8A%A0%E9%80%9F%E9%85%8D%E7%BD%AE.md)

[3] 下载Centos7.2.1511的镜像文件

```
[root@sundafei ~]# docker pull centos:7.2.1511
Trying to pull repository docker.io/library/centos ... 
7.2.1511: Pulling from docker.io/library/centos
f2d1d709a1da: Pull complete 
Digest: sha256:7c47810fd05ba380bd607a1ece3b4ad7e67f5906b1b981291987918cb22f6d4d
Status: Downloaded newer image for docker.io/centos:7.2.1511
[root@sundafei ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos    7.2.1511            0a2bad7da9b5        6 months ago        195 MB

```

[4] 运行一个容器

```
[root@sundafei ~]# docker run -it --name hdf -d docker.io/centos:7.2.1511 /bin/bash
388c77ac52deba60e62f011dc42e51258a1371423005fb6ede2094fde32c5460
[root@sundafei ~]# docker exec -it hdf /bin/bash
[root@388c77ac52de /]# 
```

### 准备环境

[1] 安装使用工具

```
[root@388c77ac52de /]# yum install wget vim lsof -y
```

[2] 安装JDK,Maven,Gradle,Node,Ant

 - 下载安装软件包

```
[root@388c77ac52de /]# cd /opt/


[root@388c77ac52de /]# wget http://192.168.0.220/source/sundafei/tools/apache-ant-1.9.9.tar.gz
--2018-05-15 15:45:51--  http://192.168.0.220/source/sundafei/tools/apache-ant-1.9.9.tar.gz
Connecting to 192.168.0.220:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5729204 (5.5M) [application/x-gzip]
Saving to: 'apache-ant-1.9.9.tar.gz'

100%[===============================================================>] 5,729,204   --.-K/s   in 0.02s   

2018-05-15 15:45:52 (230 MB/s) - 'apache-ant-1.9.9.tar.gz' saved [5729204/5729204]

可以去官网下载：https://archive.apache.org/dist/ant/binaries/

[root@388c77ac52de /]# wget http://192.168.0.220/source/sundafei/tools/apache-maven-3.3.9.tar.gz
--2018-05-15 15:46:02--  http://192.168.0.220/source/sundafei/tools/apache-maven-3.3.9.tar.gz
Connecting to 192.168.0.220:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 8575669 (8.2M) [application/x-gzip]
Saving to: 'apache-maven-3.3.9.tar.gz'

100%[===============================================================>] 8,575,669   --.-K/s   in 0.04s   

2018-05-15 15:46:02 (205 MB/s) - 'apache-maven-3.3.9.tar.gz' saved [8575669/8575669]

[root@388c77ac52de /]# wget http://192.168.0.220/source/sundafei/tools/gradle-4.0.tar.gz
--2018-05-15 15:46:08--  http://192.168.0.220/source/sundafei/tools/gradle-4.0.tar.gz
Connecting to 192.168.0.220:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 67408758 (64M) [application/x-gzip]
Saving to: 'gradle-4.0.tar.gz'

100%[===============================================================>] 67,408,758  89.5MB/s   in 0.7s   

2018-05-15 15:46:09 (89.5 MB/s) - 'gradle-4.0.tar.gz' saved [67408758/67408758]

官网下载地址：http://services.gradle.org/distributions/

[root@388c77ac52de /]# wget http://192.168.0.220/source/sundafei/tools/node-v8.9.4-linux-x64.tar.gz
--2018-05-15 15:46:15--  http://192.168.0.220/source/sundafei/tools/node-v8.9.4-linux-x64.tar.gz
Connecting to 192.168.0.220:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18188331 (17M) [application/x-gzip]
Saving to: 'node-v8.9.4-linux-x64.tar.gz'

100%[===============================================================>] 18,188,331  --.-K/s   in 0.1s    

2018-05-15 15:46:15 (156 MB/s) - 'node-v8.9.4-linux-x64.tar.gz' saved [18188331/18188331]


官网下载地址：https://nodejs.org/dist/

[root@388c77ac52de /]# wget http://archive.redoop.com/CRF/x86/centos7/utils/jdk/1.8/jdk-8u112-linux-x64.tar.gz
--2018-05-15 15:46:22--  http://archive.redoop.com/CRF/x86/centos7/utils/jdk/1.8/jdk-8u112-linux-x64.tar.gz
Resolving archive.redoop.com (archive.redoop.com)... 223.223.188.68
Connecting to archive.redoop.com (archive.redoop.com)|223.223.188.68|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 183212596 (175M) [application/x-gzip]
Saving to: 'jdk-8u112-linux-x64.tar.gz'

100%[===============================================================>] 183,212,596 97.4MB/s   in 1.8s   

2018-05-15 15:46:24 (97.4 MB/s) - 'jdk-8u112-linux-x64.tar.gz' saved [183212596/183212596]

[root@388c77ac52de opt]# ls
apache-ant-1.9.9.tar.gz    gradle-4.0.tar.gz           node-v8.9.4-linux-x64.tar.gz
apache-maven-3.3.9.tar.gz  jdk-8u112-linux-x64.tar.gz

```

 - 解压软件包并删除源文件

```
[root@388c77ac52de opt]# tar -zxf apache-ant-1.9.9.tar.gz 
[root@388c77ac52de opt]# tar -zxf apache-maven-3.3.9.tar.gz 
[root@388c77ac52de opt]# tar -zxf gradle-4.0.tar.gz 
[root@388c77ac52de opt]# tar -zxf jdk-8u112-linux-x64.tar.gz 
[root@388c77ac52de opt]# tar -zxf node-v8.9.4-linux-x64.tar.gz

[root@388c77ac52de opt]# rm -fr *.tar.gz
```

 - 更改软件目录名称

```
[root@388c77ac52de opt]# mv apache-ant-1.9.9/ ant-1.9.9
[root@388c77ac52de opt]# mv apache-maven-3.3.9/ maven-3.3.9
[root@388c77ac52de opt]# mv jdk1.8.0_112/ jdk-1.8
[root@388c77ac52de opt]# mv node-v8.9.4-linux-x64/ node-8.9.4
[root@388c77ac52de opt]# ls
ant-1.9.9  gradle-4.0  jdk-1.8  maven-3.3.9  node-8.9.4
```

 - 配置

```
[root@388c77ac52de profile.d]# vim java.sh

export JAVA_HOME=/opt/jdk-1.8
export JRE_HOME=/opt/jdk-1.8/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

```


```
[root@388c77ac52de profile.d]# vim maven.sh 

export MAVEN_HOME=/opt/maven-3.3.9
export PATH=$MAVEN_HOME/bin:$PATH

```

```
[root@388c77ac52de profile.d]# vim ant.sh 

export ANT_HOME=/opt/ant-1.9.9
export PATH=$ANT_HOME/bin:$PATH

```


```
[root@388c77ac52de profile.d]# vim gradle.sh 

export GRADLE_HOME=/opt/gradle-4.0
export PATH=$GRADLE_HOME/bin:$PATH

```


```
[root@388c77ac52de profile.d]# vim node.sh 

export NODE_HOME=/opt/node-8.9.4
export PATH=$NODE_HOME/bin:$PATH

```

 - 验证

```
[root@388c77ac52de profile.d]# source /etc/profile

[root@388c77ac52de profile.d]# node -v
v8.9.4

[root@388c77ac52de profile.d]# npm -v
5.6.0

[root@388c77ac52de profile.d]# mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T16:41:47+00:00)
Maven home: /opt/maven-3.3.9
Java version: 1.8.0_112, vendor: Oracle Corporation
Java home: /opt/jdk-1.8/jre
Default locale: en_US, platform encoding: ANSI_X3.4-1968
OS name: "linux", version: "3.10.0-327.el7.x86_64", arch: "amd64", family: "unix"

[root@388c77ac52de profile.d]# java -version
java version "1.8.0_112"yanz
Java(TM) SE Runtime Environment (build 1.8.0_112-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b15, mixed mode)

[root@388c77ac52de profile.d]# ant -version
Apache Ant(TM) version 1.9.9 compiled on February 2 2017

[root@388c77ac52de profile.d]# gradle --version

------------------------------------------------------------
Gradle 4.0
------------------------------------------------------------

Build time:   2017-06-14 15:11:08 UTC
Revision:     316546a5fcb4e2dfe1d6aa0b73a4e09e8cecb5a5

Groovy:       2.4.11
Ant:          Apache Ant(TM) version 1.9.6 compiled on June 29 2015
JVM:          1.8.0_112 (Oracle Corporation 25.112-b15)
OS:           Linux 3.10.0-327.el7.x86_64 amd64

```

 - 配置Maven的存放目录

```
[root@388c77ac52de profile.d]# cd /opt/maven-3.3.9/

[root@388c77ac52de maven-3.3.9]# mkdir working

[root@388c77ac52de maven-3.3.9]# vim conf/settings.xml


<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->

  <localRepository>/opt/maven-3.3.9/working/</localRepository>
  <!-- interactiveMode

```

[3] 安装编译及依赖

```
[root@388c77ac52de ~]# cd

[root@388c77ac52de ~]# yum install epel-release -y

[root@388c77ac52de ~]# yum install make cmake rpmdevtools rpm-build -y


[root@388c77ac52de ~]# yum install gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel tree openldap-devel -y

[root@388c77ac52de ~]# pip -V
pip 8.1.2 from /usr/lib/python2.7/site-packages (python 2.7)
```

> 配置系统编码

```
[root@914183c7c719 ~]# vim /etc/locale.conf
LANG="en_US.UTF-8"

[root@914183c7c719 ~]# vim /etc/sysconfig/i18n
LANG="en_US.UTF-8"

[root@914183c7c719 ~]# vim /etc/environment
LANG=en_US.utf-8

[root@388c77ac52de ~]# source /etc/environment
[root@388c77ac52de ~]# source /etc/sysconfig/i18n

[root@388c77ac52de ~]# localedef -v -c -i en_US -f UTF-8 en_US.UTF-8

[root@388c77ac52de ~]# locale
LANG=
LC_CTYPE="POSIX"
LC_NUMERIC="POSIX"
LC_TIME="POSIX"
LC_COLLATE="POSIX"
LC_MONETARY="POSIX"
LC_MESSAGES="POSIX"
LC_PAPER="POSIX"
LC_NAME="POSIX"
LC_ADDRESS="POSIX"
LC_TELEPHONE="POSIX"
LC_MEASUREMENT="POSIX"
LC_IDENTIFICATION="POSIX"
LC_ALL=

```

[4] 安装sbt

```
[root@388c77ac52de ~]# curl https://bintray.com/sbt/rpm/rpm |tee /etc/yum.repos.d/bintray-sbt-rpm.repo
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   160    0   160    0     0     50      0 --:--:--  0:00:03 --:--:--    50
#bintray--sbt-rpm - packages by  from Bintray
[bintray--sbt-rpm]
name=bintray--sbt-rpm
baseurl=https://sbt.bintray.com/rpm
gpgcheck=0
repo_gpgcheck=0
enabled=1

[root@388c77ac52de ~]# yum install sbt -y
```

[5] 安装Python的虚拟环境

```
[root@388c77ac52de ~]# pip install virtualenv

[root@388c77ac52de ~]# yum install autoconf libtool -y

[root@388c77ac52de ~]# yum install cppunit* -y
```

[6] 安装protobuf

```
[root@388c77ac52de ~]# cd /opt/
[root@388c77ac52de opt]# wget http://192.168.0.220/source/sundafei/tools/protobuf-2.5.0.tar.gz

[root@388c77ac52de opt]# tar -zxf protobuf-2.5.0.tar.gz 
[root@388c77ac52de opt]# rm -fr protobuf-2.5.0.tar.gz 
[root@388c77ac52de opt]# mv protobuf-2.5.0/ protobuf-2.5.0-source

[root@388c77ac52de opt]# mkdir -p protobuf-2.5.0/install
[root@388c77ac52de opt]# cd protobuf-2.5.0-source

[root@388c77ac52de protobuf-2.5.0-source]# ./configure --prefix=/opt/protobuf-2.5.0/install/

[root@388c77ac52de protobuf-2.5.0-source]# make && make install

[root@388c77ac52de protobuf-2.5.0-source]# cd .. && rm -fr protobuf-2.5.0-source

[root@388c77ac52de opt]# vim /etc/profile.d/protobuf.sh
export PROTOBUF_HOME=/opt/protobuf-2.5.0/install
export PATH=$PROTOBUF_HOME/bin:$PATH

[root@388c77ac52de opt]# source /etc/profile
[root@388c77ac52de opt]# protoc --version
libprotoc 2.5.0
```

[7] 安装编译Superset的环境

```
[root@388c77ac52de opt]# yum install lzo-devel zlib-devel autoconf automake libtool cmake -y

[root@388c77ac52de opt]# yum install  svn   ncurses-devel   gcc*  autoconf automake libtool cmake -y

[root@388c77ac52de opt]# yum install postgresql-devel mysql-devel sqlite-devel -y

[root@388c77ac52de opt]# pip install --upgrade pip
[root@388c77ac52de opt]# pip install psycopg2
[root@388c77ac52de opt]# pip install pyhive
[root@388c77ac52de opt]# pip install impyla
[root@388c77ac52de opt]# pip install mysqlclient
[root@388c77ac52de opt]# pip install sqlalchemy-clickhouse
[root@388c77ac52de opt]# pip install pygresql
[root@388c77ac52de opt]# pip install PyMySQL

[root@388c77ac52de opt]# exit
```

### 导出镜像

```
[root@study ~]# docker commit 2cde23331e5a crf-bigtop/www.redoop.com:centos-7.2
sha256:5e7ab50d247268ec78628b81da04ec2df1aa4f6571017638c65a9984660d0e61

[root@study ~]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
crf-bigtop/www.redoop.com   centos-7.2          5e7ab50d2472        17 seconds ago      3.28 GB
docker.io/centos            7.2.1511            0a2bad7da9b5        6 months ago        195 MB
```

### 镜像保存

```
[root@study ~]# docker save 5e7ab50d2472 > crf-bigtop.tar
```