
### 简要说明

> ElasticSearch6.2在Centos7.2上的安装配置,ElasticSearch是一款分布式的开源的搜索引擎框架

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：master

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.210（master）

### 系统检查

```
[root@master ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@master ~]# uname -a
Linux master 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装

> ES6.1.0要求的版本 1.8.0_131 or later

[1]安装JDK详见:：[JDK在Linux的安装配置](http://note.youdao.com/)

[2] 下载ES 版本6.1.0

```

[root@master ~]# wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.1.0.tar.gz
```

[3] 解压运行

```
[root@master ~]# tar -zxvf elasticsearch-6.1.0.tar.gz
[root@master ~]# cd elasticsearch-6.1.0/bin/
[root@master ~]# ./elasticsearch
```

[4] 启动问题1

> 报错

```
[root@master bin]# ./elasticsearch
[2017-12-19T16:03:32,074][WARN ][o.e.b.ElasticsearchUncaughtExceptionHandler] [] uncaught exception in thread [main]
org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:125) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:112) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124) ~[elasticsearch-cli-6.1.0.jar:6.1.0]
	at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:85) ~[elasticsearch-6.1.0.jar:6.1.0]
Caused by: java.lang.RuntimeException: can not run elasticsearch as root
	at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:104) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:171) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:322) ~[elasticsearch-6.1.0.jar:6.1.0]
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:121) ~[elasticsearch-6.1.0.jar:6.1.0]
	... 6 more
```

> 问题：

```
不能使用root用户运行
```

> 创建es用户

```
[root@master bin]# useradd es
[root@master bin]# su es
[root@master bin]# ./elasticsearch
```

[5] 启动问题2

> 报错

```
[root@master bin]# su - es
[es@master bin]$ ./elasticsearch
grep: /opt/install/elasticsearch-6.1.0/config/jvm.options: Permission denied
Exception in thread "main" 2017-12-19 16:04:25,365 main ERROR No log4j2 configuration file found. Using default configuration: logging only errors to the console. Set system property 'log4j2.debug' to show Log4j2 internal initialization logging.
2017-12-19 16:04:25,598 main ERROR Could not register mbeans java.security.AccessControlException: access denied ("javax.management.MBeanTrustPermission" "register")
	at java.security.AccessControlContext.checkPermission(AccessControlContext.java:472)
	at java.lang.SecurityManager.checkPermission(SecurityManager.java:585)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.checkMBeanTrustPermission(DefaultMBeanServerInterceptor.java:1848)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.registerMBean(DefaultMBeanServerInterceptor.java:322)
    ......
	at java.io.PrintStream.println(PrintStream.java:821)
	at java.lang.Throwable$WrappedPrintStream.println(Throwable.java:748)
	at java.lang.Throwable.printStackTrace(Throwable.java:655)
	at java.lang.Throwable.printStackTrace(Throwable.java:643)
	at java.lang.ThreadGroup.uncaughtException(ThreadGroup.java:1061)
	at java.lang.ThreadGroup.uncaughtException(ThreadGroup.java:1052)
	at java.lang.Thread.dispatchUncaughtException(Thread.java:1956)

SettingsException[Failed to load settings from /opt/install/elasticsearch-6.1.0/config/elasticsearch.yml]; nested: AccessDeniedException[/opt/install/elasticsearch-6.1.0/config/elasticsearch.yml];
	at org.elasticsearch.node.InternalSettingsPreparer.prepareEnvironment(InternalSettingsPreparer.java:101)
	at org.elasticsearch.cli.EnvironmentAwareCommand.createEnv(EnvironmentAwareCommand.java:95)
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86)
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124)
	at org.elasticsearch.cli.Command.main(Command.java:90)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:85)
Caused by: java.nio.file.AccessDeniedException: /opt/install/elasticsearch-6.1.0/config/elasticsearch.yml
	at sun.nio.fs.UnixException.translateToIOException(UnixException.java:84)
	at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:102)
	at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:107)
	at sun.nio.fs.UnixFileSystemProvider.newByteChannel(UnixFileSystemProvider.java:214)
	at java.nio.file.Files.newByteChannel(Files.java:361)
	at java.nio.file.Files.newByteChannel(Files.java:407)
	at java.nio.file.spi.FileSystemProvider.newInputStream(FileSystemProvider.java:384)
	at java.nio.file.Files.newInputStream(Files.java:152)
	at org.elasticsearch.common.settings.Settings$Builder.loadFromPath(Settings.java:1161)
	at org.elasticsearch.node.InternalSettingsPreparer.prepareEnvironment(InternalSettingsPreparer.java:99)
	... 6 more
```

> 问题：

```  
es用户没有权限运行
```

> 授权

```
使用root用户对es整个文件目录授权更改用户组
[es@master bin]$ exit
[es@master bin]$ cd ../../
[root@master ~]# chown -R es:es elasticsearch-6.1.0
```

> 运行成功

```
[es@master bin]$ ./elasticsearch
[2017-12-19T16:06:17,473][INFO ][o.e.n.Node               ] [] initializing ...
[2017-12-19T16:06:17,638][INFO ][o.e.e.NodeEnvironment    ] [Q5l6j50] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [292.8gb], net total_space [294.3gb], types [rootfs]

......

[2017-12-19T16:06:26,439][INFO ][o.e.c.s.ClusterApplierService] [Q5l6j50] new_master {Q5l6j50}{Q5l6j503Qj6y9HmNVoZ25Q}{ul5Uwm9lSR6SalCUa996uQ}{127.0.0.1}{127.0.0.1:9300}, reason: apply cluster state (from master [master {Q5l6j50}{Q5l6j503Qj6y9HmNVoZ25Q}{ul5Uwm9lSR6SalCUa996uQ}{127.0.0.1}{127.0.0.1:9300} committed version [1] source [zen-disco-elected-as-master ([0] nodes joined)]])
[2017-12-19T16:06:26,471][INFO ][o.e.h.n.Netty4HttpServerTransport] [Q5l6j50] publish_address {127.0.0.1:9200}, bound_addresses {[::1]:9200}, {127.0.0.1:9200}
[2017-12-19T16:06:26,471][INFO ][o.e.n.Node               ] [Q5l6j50] started
[2017-12-19T16:06:26,485][INFO ][o.e.g.GatewayService     ] [Q5l6j50] recovered [0] indices into cluster_state
```

### 结果验证

```
[root@master bin]# curl -XGET 'localhost:9200/?pretty'
{
  "name" : "Q5l6j50",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "NSH4rgSNTAq61-RXM2U7BA",
  "version" : {
    "number" : "6.1.0",
    "build_hash" : "c0c1ba0",
    "build_date" : "2017-12-12T12:32:54.550Z",
    "build_snapshot" : false,
    "lucene_version" : "7.1.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

[1] 访问问题1

> 问题

```
外网不能访问
```

> 解决

```
[root@master bin]# cd ../config/
[root@master config]# vim elasticsearch.yml
.......
# Set the bind address to a specific IP (IPv4 or IPv6):
#
#network.host: 192.168.0.1
network.host: 0.0.0.0
```

> 验证

```
[root@master config]# cd ../bin/
[root@master bin]# su - es 
[es@master bin]$ ./elasticsearch
[2017-12-26T15:26:36,785][INFO ][o.e.n.Node               ] [master] initializing ...
[2017-12-26T15:26:36,959][INFO ][o.e.e.NodeEnvironment    ] [master] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [190.1gb], net total_space [191.5gb], types [rootfs]

......

[2017-12-26T15:26:43,105][INFO ][o.e.n.Node               ] [master] starting ...
[2017-12-26T15:26:43,537][INFO ][o.e.t.TransportService   ] [master] publish_address {192.168.1.210:9300}, bound_addresses {[::]:9300}
[2017-12-26T15:26:43,550][INFO ][o.e.b.BootstrapChecks    ] [master] bound or publishing to a non-loopback or non-link-local address, enforcing bootstrap checks
ERROR: [2] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2017-12-26T15:26:43,560][INFO ][o.e.n.Node               ] [master] stopping ...
[2017-12-26T15:26:43,620][INFO ][o.e.n.Node               ] [master] stopped
[2017-12-26T15:26:43,621][INFO ][o.e.n.Node               ] [master] closing ...
[2017-12-26T15:26:43,660][INFO ][o.e.n.Node               ] [master] closed

```

[2] 修改配置之后启动不起来

> 问题：

    ES已经启动不起来了，可以看到有两个地方报错

```
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

> 解决问题1

这个是linux下常见的错误，主要是因为linux会限制进程的最大打开文件数，只需要简单配置一下即可解决，在ES的官方文档中提供了两种解决方案。

系统基本的配置方式：

```
[root@study elasticsearch-6.1.0]# vim /etc/security/limits.conf
......

#<domain>      <type>  <item>         <value>
#

#*               soft    core            0
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#@student        -       maxlogins       4
es               -       nofile          65536

# End of file

```
保存修改
    说明这里 es 表明这个配置只对kael用户生效，如果你用来启动elasticsearch的用户名不是这个，那么你需要按你实际的用户名修改

> 解决问题2
这个是ES使用的虚拟内存,

```
[root@study elasticsearch-6.1.0]# sysctl -w vm.max_map_count=262144
vm.max_map_count = 262144
调大虚拟内存即可。
```

> 验证

```
[es@study bin]$ ./elasticsearch
[2017-12-26T16:08:18,561][INFO ][o.e.n.Node               ] [master] initializing ...
[2017-12-26T16:08:18,727][INFO ][o.e.e.NodeEnvironment    ] [master] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [190.1gb], net total_space [191.5gb], types [rootfs]

......

[2017-12-26T16:08:24,538][INFO ][o.e.n.Node               ] [master] starting ...
[2017-12-26T16:08:27,243][INFO ][o.e.t.TransportService   ] [master] publish_address {192.168.1.210:9300}, bound_addresses {[::]:9300}
[2017-12-26T16:08:27,267][INFO ][o.e.b.BootstrapChecks    ] [master] bound or publishing to a non-loopback or non-link-local address, enforcing bootstrap checks
[2017-12-26T16:08:30,395][INFO ][o.e.c.s.MasterService    ] [master] zen-disco-elected-as-master ([0] nodes joined), reason: new_master {master}{hFk2XKoiTIeIDVzAM1r31w}{I45XGHL_RO6T9iugFHnzyQ}{192.168.1.210}{192.168.1.210:9300}
[2017-12-26T16:08:30,405][INFO ][o.e.c.s.ClusterApplierService] [master] new_master {master}{hFk2XKoiTIeIDVzAM1r31w}{I45XGHL_RO6T9iugFHnzyQ}{192.168.1.210}{192.168.1.210:9300}, reason: apply cluster state (from master [master {master}{hFk2XKoiTIeIDVzAM1r31w}{I45XGHL_RO6T9iugFHnzyQ}{192.168.1.210}{192.168.1.210:9300} committed version [1] source [zen-disco-elected-as-master ([0] nodes joined)]])
[2017-12-26T16:08:30,462][INFO ][o.e.h.n.Netty4HttpServerTransport] [master] publish_address {192.168.1.210:9200}, bound_addresses {[::]:9200}
[2017-12-26T16:08:30,463][INFO ][o.e.n.Node               ] [master] started
[2017-12-26T16:08:30,467][INFO ][o.e.g.GatewayService     ] [master] recovered [0] indices into cluster_state
```

> 最后一步

```
[root@study config]# systemctl stop firewalld
[root@study config]# systemctl disable firewalld
```

> 重新启动，就可以正常访问了，现在我们可以在任何机器上访问ES了。
> 也可以用后台进程的方式启动：

```
[es@study bin]$ ./elasticsearch -d -p pid
```