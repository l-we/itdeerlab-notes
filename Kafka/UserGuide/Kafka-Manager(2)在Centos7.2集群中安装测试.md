### 简要说明

> Kafka-Manager测试，紧接上节中Kafka-Manager源码编译之后，进行测试

### 环境准备

 - 2台Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：master & node

 > 两台机器安装好了HDP的集群

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

> 由于资源的问题，我的集群只启动了Zookeeper和Kafka两个组件

![2018621135135](http://panrhkqz9.bkt.clouddn.com/2018621135135.png)
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

kafka-manager.zkhosts="kafka-manager-zookeeper:2181"
改为：
kafka-manager.zkhosts="192.168.1.81:2181,192.168.1.82:2181"
kafka-manager.zkhosts=${?ZK_HOSTS}
pinned-dispatcher.type="PinnedDispatcher"
```

[4] 启动

```
[root@master kafka-manager-1.3.3.16]# ./bin/kafka-manager

直接就可以启动，但是我这里使用的是2个节点的集群，Kafka-Manager默认启动的端口9000已经被占用了，所以这里我启动时指定端口号。
[root@master kafka-manager-1.3.3.16]# ./bin/kafka-manager -Dhttp.port=9500

07:42:38,339 |-INFO in ch.qos.logback.classic.LoggerContext[default] - Could NOT find resource [logback.groovy]
07:42:38,339 |-INFO in ch.qos.logback.classic.LoggerContext[default] - Could NOT find resource [logback-test.xml]
......
[info] play.api.Play - Application started (Prod)
[info] p.c.s.NettyServer - Listening for HTTP on /0:0:0:0:0:0:0:0:9500
[info] k.m.a.KafkaManagerActor - Updating internal state...
```

> 后台启动

```
[root@master kafka-manager-1.3.3.16]# nohup /opt/install/kafka-manager-1.3.3.16/bin/kafka-manager -Dconfig.file=/opt/install/kafka-manager-1.3.3.16/conf/application.conf &
```

[5] 前端检查

 - 防火墙请自动关闭，或者做防火墙策略

 - 对selinux做配置

 - 在浏览器输入你部署Kafka-Manager的机器IP:9500访问

![2018621135241](http://panrhkqz9.bkt.clouddn.com/2018621135241.png)

> 由此能看到前端能正常访问说明Kafka-Manager已经能正常启动起来了。下一节介绍Kafka-Manager的简单使用。