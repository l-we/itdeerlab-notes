### 简要说明

> JDK在Linux下的安装配置（jdk7 or jdk8）前提:已经有一台Linux操作系统，切使用root用户登录,有wget和vim命令
    
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

> 这里说明一下，Centos7.2的环境，默认安装完成就是有OpenJDK的，直接配置自己的OracleJDK就能覆盖掉OpenJDK，OpenJDK在很多应用运行环境中是没有问题的，但是有些编译环境还是有些小的问题，这里建议直接安装Oracle的JDK。下面简述OpenJDK的卸载。

[1] OpenJDK卸载

 - 检查

```
[root@study ~]# java -version
openjdk version "1.8.0_65"
OpenJDK Runtime Environment (build 1.8.0_65-b17)
OpenJDK 64-Bit Server VM (build 25.65-b01, mixed mode)
```

 - 显示安装包

```
[root@study ~]# rpm -qa | grep openjdk
java-1.8.0-openjdk-headless-1.8.0.65-3.b17.el7.x86_64
java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64
java-1.7.0-openjdk-1.7.0.91-2.6.2.3.el7.x86_64
java-1.7.0-openjdk-headless-1.7.0.91-2.6.2.3.el7.x86_64
```

 - 卸载

```
[root@study ~]# yum -y remove java-1.8.0-openjdk-headless-1.8.0.65-3.b17.el7.x86_64
[root@study ~]# yum -y remove java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64
[root@study ~]# yum -y remove java-1.7.0-openjdk-1.7.0.91-2.6.2.3.el7.x86_64
[root@study ~]# yum -y remove java-1.7.0-openjdk-headless-1.7.0.91-2.6.2.3.el7.x86_64
```

[2] JDK下载

```
[root@study ~]# mkdir -p /opt/install
[root@study ~]# cd /opt/install
[root@study install]# wget http://192.168.0.220/source/jdk/jdk-8u112-linux-x64.tar.gz

注意： 我这里是自己搭建的源服务器，JDK的包从其他地方或者官网下载即可，或者本地上传也可以
官网下载地址:http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```

[3] 解压

```
[root@study install]# tar -zxvf jdk-8u*.tar.gz
[root@study install]# mv jdk1.8.0* jdk1.8
[root@study install]# rm -fr jdk-8u*.tar.gz
```

[4] 配置

```
[root@study install]# vi /etc/profile.d/java.sh

export JAVA_HOME=/opt/install/jdk1.8
export JRE_HOME=/opt/install/jdk1.8/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

[5] 使配置生效

```
[root@study install]# source /etc/profile
```

### 结果验证

```
[root@study install]# java -version

java version "1.8.0_112"
Java(TM) SE Runtime Environment (build 1.8.0_112-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b15, mixed mode)
```

> 配置完成，JDK解压版的都可以这样配置版本不限制