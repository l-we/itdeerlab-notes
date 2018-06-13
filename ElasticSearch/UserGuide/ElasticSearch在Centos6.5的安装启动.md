
> 在Linux环境中安装ElasticSearch高速搜索软件,ElasticSearch是一个基于Lucene的构建的开源的，分布式的，RESTful接口全文搜索引擎，还可以把她理解为分布式的文档数据库。

> 前提:已经有一台Linux操作系统，切使用root用户登录
    有wget和vim命令
    
### 安装环境

> Centos6.5 x86 64位 最小化安装

个人准备了两台Centos6.5的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.0.200（master）192.168.0.201（node）

### 查看环境

```
[root@master ~]# cat /etc/redhat-release 
CentOS release 6.5 (Final)
[root@node ~]# cat /etc/redhat-release 
CentOS release 6.5 (Final)
```

### JDK安装

1. 监测Linux机器上是否已经配置好了JDK

```
[root@master ~]# java -version
显示以下信息说明已经安装配置完成
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
[root@master ~]# java -version
显示以下信息说明没有安装配置
-bash: java: command not found
```
2. 上传解压jdk

```
[root@master ~]# mkdir -p /opt/install
把jdk的安装包上传到Linux操作系统的/opt/install目录下
解压jdk-7u79-linux-x64.tar.gz，解压文件是jdk1.7.0_79
tar -zxvf jdk-7u79-linux-x64.tar.gz
给解压文件jdk1.7.0_79更改名称为jdk
mv  jdk1.7.0_79  jdk
```

3. 配置环境变量

```
vim  /etc/profile.d/jdk.sh
插入一下语句
export JAVA_HOME=/opt/install/jdk
export JRE_HOME=/opt/install/jdk/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

4. 使生效

```
source  /etc/profile	 使配置文件生效
java  -version	 验证
```

### Elasticsearch下载

1.下载安装包

```
官网下载地址：https://www.elastic.co/downloads/past-releases/elasticsearch-5-0-0
选择ZIP或者TAR包都是可以的
```

2.上传ES压缩包，并解压

```
[root@master ~]# mkdir -p /opt/install
[root@master ~]# tar zxvf elasticsearch-5.0.0.tar.gz -C /opt/install
[root@master ~]# cd /opt/install
[root@master ~]# mv elasticsearch-5.0.0  es
```

3.修改配置文件

```
[root@master ~]# cat /opt/es/config/elasticsearch.yml
1）先修改网络配置
network.host: 0.0.0.0 
http.port: 9200
下面两个如果没有就自己添加
http.cors.enabled:  true
http.cors.allow-origin:  "*"
2）修改集群配置(master机器的配置)
# cluster
cluster.name:  "es-cluster"
node.name:  "master"
node.master:  true
node.data:  true
http.enabled:  true
```
4.修改系统配置

```
[root@master ~]# cat /etc/security/limits.conf
* soft nofile 65536 
* hard nofile 65536
* soft nproc 2048 
* hard nproc 4096

[root@master ~]# cat /etc/sysctl.conf
vm.max_map_count = 655360
如果没有执行下面命令：
echo "vm.max_map_count=262144" >>/etc/sysctl.conf

查看系统配置文件
[root@master ~]# sysctl -p
```
PS :如果不修改以上配置，network.host: localhost 只能绑定localhost或者127.0.0.1，不能绑定外网IP，还会出现以下错误提示

```
max file descriptors [65535] for elasticsearch process likely too low, increase to at least [65536]

max number of threads [1024] for user [www] likely too low, increase to at least [2048]

max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```

### 集群节点node配置

1）拷贝
```
[root@master ~]# cp -r es 192.168.0.201:/opt/install
```
2）修改配置
```
[root@master ~]# cat /opt/install/es/config/elasticsearch.yml 
网络：
network.host: 0.0.0.0 
http.port: 9201

http.cors.enabled:  true
http.cors.allow-origin:  "*"

集群：
# cluster
cluster.name:  "es-cluster"
node.name:  "node"
node.master:  true
node.data:  true
http.enabled:  true
```
PS :只需修改http.port和node.name

### 启动集群

1.创建用户

```
#useradd es
```

2.修改es文件权限

```
[root@master ~]# chown es:es -R /opt/install/es
[root@node ~]# chown es:es -R /opt/install/es
```

3.切换用户并启动

```
[root@master ~]# su - www -c "/opt/install/es/bin/elasticsearch >/dev/null 2>&1 &"
[root@node ~]# su - www -c "/opt/install/es/bin/elasticsearch >/dev/null 2>&1 &"
```
elasticsearch不支持root直接启动。
4. 查看端口

```
[root@master ~]# netstat -ntlp 
tcp        0      0 0.0.0.0:9200                0.0.0.0:*                   LISTEN      29660/java              
tcp        0      0 0.0.0.0:9201                0.0.0.0:*                   LISTEN      29757/java         
tcp        0      0 0.0.0.0:9300                0.0.0.0:*                   LISTEN      29660/java         
tcp        0      0 0.0.0.0:9301                0.0.0.0:*                   LISTEN      29757/java
```
5.web访问

```
[root@master ~]#  curl http://192.168.1.200:9200

{
   "name"   :  "master" ,

   "cluster_name"   :  "es-cluster" ,

   "cluster_uuid"   :  "2C5tWrgISW6V-SAX203LbQ" ,

   "version"   : {

     "number"   :  "5.0.0" ,

     "build_hash"   :  "253032b" ,

     "build_date"   :  "2016-10-26T04:37:51.531Z" ,

     "build_snapshot"   :  false ,

     "lucene_version"   :  "6.2.0"

   },

   "tagline"   :  "You Know, for Search"

}
```


```
[root@node ~]# curl  http://192.168.1.201:9200

   "name"   :  "node" ,

   "cluster_name"   :  "es-cluster" ,

   "cluster_uuid"   :  "2C5tWrgISW6V-SAX203LbQ" ,

   "version"   : {

     "number"   :  "5.0.0" ,

     "build_hash"   :  "253032b" ,

     "build_date"   :  "2016-10-26T04:37:51.531Z" ,

     "build_snapshot"   :  false ,

     "lucene_version"   :  "6.2.0"

   },

   "tagline"   :  "You Know, for Search"

}
```

### 常见问题

1. 若访问不到，则需要关闭防火墙
```
[root@master ~]# /etc/init.d/iptables stop
[root@master ~]# /etc/init.d/iptables disable
[root@master ~]# /etc/init.d/iptables status

```

2.安装出错一
```
max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536]
```
> 解决方法：切换到root用户，进入

```
vim /etc/security/limits.conf  ，增加配置（保存后注意切回普通用户的时候才能生效，sudo 修改的不能立即生效）：
work soft nofile 819200  
work hard nofile 819200 
```



3.安装出错二

```
max number of threads [1024] for user [work] likely too low, increase to at least [2048]
```
> 解决方法：进入limits.d下的配置文件：vi /etc/security/limits.d/90-nproc.conf ，修改配置如下：

```
*          soft    nproc     1024  
修改为：  
*          soft    nproc     2048  
```
3. 安装出错三
 
```
max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
```
> 解决方法：修改sysctl文件：vi /etc/sysctl.conf ，增加下面配置项：

```
增加改行配置：vm.max_map_count=655360  
保存退出后，执行：  
sysctl -p  
```
> 另外再配置ES的时候，threadpool.bulk.queue_size 已经变成了thread_pool.bulk.queue_size ，ES_HEAP_SIZE，ES_MAX_MEM等配置都变为ES_JAVA_OPTS这一配置项，如限制内存最大大小为1G：

```
export ES_JAVA_OPTS="-Xms1g -Xmx1g"
```

> 至此ES两个节点的集群搭建完成，可以进行搜索使用。