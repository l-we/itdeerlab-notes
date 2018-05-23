### 简要说明

> Kafka-Manager测试，紧接上节中Kafka-Manager源码编译之后，进行测试

### 环境准备

 - 2台Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：master node

 > 两天机器安装好了HDP的集群

 HDP企业级集群搭建请参考[HDP-2.6企业级实施](https://github.com/ItdeerLab/itdeerlab-notes/tree/notes/HDP/UserGuide)

### 系统检查

 - 机器检测

```
[root@master ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
[root@master ~]# uname -a
Linux master 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

 - 集群检测


### Kafka-Manager测试

[1] 把源码编译的包上传至集群服务器

```
[root@master opt]# ls
kafka-manager-1.3.3.16.zip
```

[2] 解压

```
[root@master opt]# unzip kafka-manager-1.3.3.16.zip
```

[3] 配置

```
[root@master opt]# cd kafka-manager-1.3.3.16
[root@master kafka-manager-1.3.3.16]# vim conf/application.conf

kafka-manager.zkhosts="192.168.1.81:2181,192.168.1.82:2181"
kafka-manager.zkhosts=${?ZK_HOSTS}
pinned-dispatcher.type="PinnedDispatcher"

```

[4] 启动

```
[root@master kafka-manager-1.3.3.16]# ./bin/kafka-manager

直接就可以启动，但是我这里使用的是2个节点的集群，Kafka-Manager默认启动的端口已经被占用了，所以这里我启动时指定端口号。


```





