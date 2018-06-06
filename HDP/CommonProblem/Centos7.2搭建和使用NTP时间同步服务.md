
### bigdata-server-1.2系统搭建与使用NTP时间服务器

> 这几天安装一个大数据集群，安装的时候同步了网络时间，晚上的时候把服务器关机了，
> 第二天开机之后，有一些组件启动不起来，最后发现时间错乱了，故搭建了一台内网的NTP服务器，
> 把整个的学习搭建过程记录了下来，供大家参考学习，有什么不对的地方敬请指出，我会加以改正。

### 环境准备

 - bigdata-server-1.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：bigdata-server-1

> 找集群中的一台机器搭建NTP服务器，这里选择第一台机器，其他的机器同步这台机器的时间

### 系统检查

```
[root@bigdata-server-1 ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@bigdata-server-1 ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### NTP搭建环境准备

1. 关闭防火墙

> 查看防火墙的运行状态

```
[root@bigdata-server-1 ~]# systemctl status firewalld.service | grep active
   Active: active (running) since Wed 2017-07-26 20:15:42 CST; 9min ago
```

> 关闭防火墙

```
[root@bigdata-server-1 ~]# systemctl stop firewalld.service
```

> 再次检查防火墙的运行状态

```
[root@bigdata-server-1 ~]# systemctl status firewalld.service | grep active
   Active: inactive (dead) since Wed 2017-07-26 20:30:00 CST; 6min ago
```
> 禁用防火墙

```
[root@bigdata-server-1 ~]# systemctl disable firewalld.service

Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.
```
2. 禁用SELINUX

> 查看selinux的配置

```
[root@bigdata-server-1 ~]# cat /etc/selinux/config | grep SELINUX=

# SELINUX= can take one of these three values:
SELINUX=enforcing
```

> 修改selinux的配置

```
[root@bigdata-server-1 ~]# sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

> 再次查看selinux的配置

```
[root@bigdata-server-1 ~]# cat /etc/selinux/config | grep SELINUX=

# SELINUX= can take one of these three values:
SELINUX=disabled
```

> 使配置生效

```
[root@bigdata-server-1 ~]# setenforce 0
```

### 安装NTP服务器

> 检测ntp是否安装

```
[root@bigdata-server-1 ~]# rpm -qa | grep ntp
[root@bigdata-server-1 ~]# rpm -qa | grep ntpdate

没有任何输出则没有安装
```

> 安装ntp软件，默认ntpdate也会安装上

```
[root@bigdata-server-1 ~]# yum install ntp -y
```

> 查看安装情况

```
[root@bigdata-server-1 ~]# rpm -qa | grep ntp
ntp-4.2.6p5-25.el7.centos.2.x86_64
ntpdate-4.2.6p5-25.el7.centos.2.x86_64
```

### 修改配置

> NTP时间服务器本身的时间有两种：
一种是，使用的国家或国际的时间服务器为标准（所说的网络时间），本机同步了网络时间之后，再提供给内网的服务器进行时间同步。
另一种是，以本机自身的时间为标准，提供给内网的服务器进行时间的同步，让内网的服务器和本机保持一致，就当是时间同步。

```
[root@bigdata-server-1 ~]# vim /etc/ntp.conf
```

1. 首先是配置允许哪些机器，可以访问这台时间服务器

> ntp默认配置是拒绝所有的IP访问

```
# Permit time synchronization with our time source, but do not
# permit the source to query or modify the service on this system.
restrict default nomodify notrap nopeer noquery
```

(A). 配置让所有的IP都能访问（公开给其他服务器使用）

```
# restrict default nomodify notrap nopeer noquery
注释掉以上这一行，在这个配置文件的最后一行带restrict关键词的这行之后添加

restrict default modify notrap

覆盖掉之前的任何配置
```

(B). 配置让部分IP能访问（公开给在同一个IP段的机器使用）

```
#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap
把这一行的注释打开
然后修改成需要允许的IP段
如：restrict 192.168.0.0 mask 255.255.255.0 nomodify notrap
允许192.168.0网段的IP访问
```

2. 配置网络授时服务器（使用网络时间必须要保证机器是能连接网络）

```
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst

可以删除掉也可以保留

同时在下面加上一些国内常用的授时服务器地址，从网上可以找到很多
国家授时中心的NTP地址是：210.72.145.39
#指定本ntp服务器的上游ntp服务器为210.72.145.44，并且设置为首先服务器。
同步时间为：从上到下，写的越靠上，优先级越高。（此服务器同步不了时间，寻找下一个ntp服务器）。
此IP地址是中国国家授时中心ntp服务器。
server s1b.time.edu.cn iburst   #清华大学
或者
server s1c.time.edu.cn iburst   #北京大学
```

3. 配置本地的时间（服务器完全不能联网的情况下）

```
如果外部的时间服务器不可用，可以使用本地的时间

server 127.127.1.0      　　　　 
    #local clock 如果上面的服务器都无法同步时间，就和本地系统时间同步。127.127.1.0在这里是一个IP地址，不是网段。
fudge 127.127.1.0 stratum 10    
    #127.127.1.0 为第10层。ntp 和127.127.1.0同步完后，就变成了11层。  ntp是层次阶级的。同步上层服务器的stratum 大小不能超过或等于16。
```

### 启动NTP

> 查看启动情况

```
[root@bigdata-server-1 ~]# systemctl status ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```

> 启动NTP服务

```
[root@bigdata-server-1 ~]# systemctl start ntpd.service
```

> 再次查看运行情况

```
[root@bigdata-server-1 ~]# systemctl status ntpd.service | grep active

   Active: active (running) since Thu 2017-07-27 02:02:31 CST; 8h left
```

> 设置开机启动

```
[root@bigdata-server-1 ~]# systemctl enable  ntpd.service

Created symlink from /etc/systemd/system/multi-user.target.wants/ntpd.service to /usr/lib/systemd/system/ntpd.service.
```

> 同步时间

```
首先我先把时间改一下

[root@bigdata-server-1 ~]# date -s "2016-5-2  10:36:00"
Mon May  2 10:36:00 CST 2016

然后把ntp服务重新启动
[root@bigdata-server-1 ~]# systemctl restart ntpd.service

然后查看时间

[root@bigdata-server-1 ~]# date
Wed Jul 26 17:11:19 CST 2017

可以看到时间已经改过来了
```

> 验证服务

```
[root@bigdata-server-1 ~]# ntpdate -q 210.72.145.39
server 210.72.145.39, stratum 0, offset 0.000000, delay 0.00000
26 Jul 17:16:49 ntpdate[3661]: no server suitable for synchronization found

出现no server suitable for synchronization found的问题
```

```
查看用的是哪一个时间授时IP
[root@bigdata-server-1 ~]# ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+61.161.155.29   202.118.1.46     2 u   50   64    7   27.896   18.055  15.110
+ntp3.itcomplian 5.103.128.88     3 u   42   64   15  372.529    9.356   9.458
*ntp.ix.ru       .PPS.            1 u   18   64   17  179.128    2.641  13.428
-ntp2.itcomplian 5.103.128.88     3 u   13   64   17  299.383   71.462  13.442
 210.72.145.39   .STEP.          16 u    -   64    0    0.000    0.000   0.000
[root@bigdata-server-1 ~]# date -R
Wed, 26 Jul 2017 17:15:57 +0800
```

```
[root@bigdata-server-1 ~]# watch ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+61.161.155.29   202.118.1.46     2 u   50   64    7   27.896   18.055  15.110
+ntp3.itcomplian 5.103.128.88     3 u   42   64   15  372.529    9.356   9.458
*ntp.ix.ru       .PPS.            1 u   18   64   17  179.128    2.641  13.428
-ntp2.itcomplian 5.103.128.88     3 u   13   64   17  299.383   71.462  13.442
 210.72.145.39   .STEP.          16 u    -   64    0    0.000    0.000   0.000
 
 能看到第一行的 when的值在变化，说明这里使用的是61.161.155.29在做时间同步
 
```

```
这时可以使用命令进行同步
[root@bigdata-server-1 ~]# ntpdate  202.118.1.46
26 Jul 17:25:50 ntpdate[3761]: step time server 61.161.155.29 offset 38904061.670684 sec

[root@bigdata-server-1 ~]# date
Wed Jul 26 17:26:22 CST 2017
```

### 使用自己搭建的NTP时间服务器

> 在内网有一台服务器需要同步时间

```
这台服务器现在的时间是
[root@bigdata-server-1 ~]# date
Mon May  2 10:36:02 CST 2016

IP是192.168.0.81
```

> 装ntpdate

```
[root@bigdata-server-1 ~]# rpm -qa | grep ntpdate
ntpdate-4.2.6p5-25.el7.centos.2.x86_64
若没有安装
[root@bigdata-server-1 ~]# yum install ntpdate -y
```

> 同步时间

```
[root@bigdata-server-1 ~]# ntpdate 192.168.0.145
```

> 设置定时同步

```
echo "*/1 * * * * root /usr/sbin/ntpdate 192.168.0.145 && /sbin/hwclock -w" >> /etc/crontab
```
> 重启定时任务服务


```
[root@bigdata-server-1 ~]# systemctl restart crond
```

> 定时任务配置说明

```
*/1 * * * * root /usr/sbin/ntpdate 192.168.0.145 && /sbin/hwclock -w #表示每一分钟同步一次
```

00 01 * * *  五个字符表示  分 时 日 月 年，root表示用root用户执行
> 