### Centos7.2系统搭建镜像离线源

> 很多时候在客户现场安装大数据集群时，是在一个内网中，不能连接到外网中的。
> 这时候安装源或者系统源就需要自己搭建，如果不搭建安装源，可能会依赖系统的包，会报错。
> 所以要手动搭建系统和安装源，做好依赖的准备，在安装过程中就不会有那么多的问题。

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：bigdata-server-1

> 找集群中的一台机器搭建离线源，这里选择第一天台机器

### 系统检查

```
[root@bigdata-server-1 ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@bigdata-server-1 ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 模拟内网的环境

> 把DNS注释掉，不能访问外网（我这里是能连接外网的，为了模拟内网环境，需要把DNS去掉）


```
[root@bigdata-server-1 ~]# sed -i 's/DNS1/#DNS1/g' /etc/sysconfig/network-scripts/ifcfg-em1
[root@bigdata-server-1 ~]# sed -i 's/DNS2/#DNS2/g' /etc/sysconfig/network-scripts/ifcfg-em1
[root@bigdata-server-1 ~]# systemctl restart network.service
[root@bigdata-server-1 ~]# ping www.baidu.com
ping: unknown host www.baidu.com
```

### 上传镜像及安装源

> 把镜像及安装源上传到/opt目录下


```
[root@bigdata-server-1 ~]# ls /opt/
CentOS-7-x86_64-DVD-1511.iso  hdp.tar.gz
[root@bigdata-server-1 ~]# mkdir -p /opt/os
[root@bigdata-server-1 ~]# mount -o loop -t iso9660 /opt/CentOS-7-x86_64-DVD-1511.iso /opt/os
mount: /dev/loop0 is write-protected, mounting read-only

```
> 把现有的repo文件进行备份

```
[root@bigdata-server-1 ~]# mkdir -p /etc/yum.repos.d/bak
[root@bigdata-server-1 ~]# mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/bak
[root@bigdata-server-1 ~]# ls /etc/yum.repos.d/
bak
```

> 配置本地的repo源文件

```
[root@bigdata-server-1 ~]# vi /etc/yum.repos.d/CentOS7.repo

[CentOS7] 
name=Centos7 Base
baseurl=file:///opt/os
enabled=1 
gpgcheck=1 
gpgkey=file:///opt/os/RPM-GPG-KEY-CentOS-7
```

> 清除yum源的缓存

```
[root@bigdata-server-1 ~]# yum clean all
Loaded plugins: fastestmirror
Cleaning repos: CentOS7
Cleaning up everything

[root@bigdata-server-1 ~]# yum makecache

Loaded plugins: fastestmirror
CentOS7                                                                            | 3.6 kB  00:00:00     
(1/4): CentOS7/group_gz                                                            | 155 kB  00:00:00     
(2/4): CentOS7/filelists_db                                                        | 2.9 MB  00:00:00     
(3/4): CentOS7/primary_db                                                          | 2.8 MB  00:00:00     
(4/4): CentOS7/other_db                                                            | 1.2 MB  00:00:00     
Determining fastest mirrors
Metadata Cache Created
```

### 安装Http服务器

> 安装http软件（启动及设置开机启动）

```
[root@bigdata-server-1 ~]# yum install httpd -y

[root@bigdata-server-1 ~]# systemctl start httpd

[root@bigdata-server-1 ~]# systemctl enable httpd.service

Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
```

> 安装一些其他软件方便检查和使用

```
[root@bigdata-server-1 ~]# yum install wget vim lsof -y

[root@bigdata-server-1 ~]# lsof -i :80

COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
httpd   18939   root    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
httpd   18940 apache    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
httpd   18941 apache    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
httpd   18942 apache    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
httpd   18943 apache    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
httpd   18944 apache    4u  IPv6  47802      0t0  TCP *:http (LISTEN)
```

> 解压源文件挂载镜像文件

```
[root@bigdata-server-1 ~]# umount /opt/os/
[root@bigdata-server-1 ~]# ll /opt/os/
total 0
[root@bigdata-server-1 ~]# mv /opt/hdp.tar.gz /var/www/html/
[root@bigdata-server-1 ~]# mv /opt/CentOS-7-x86_64-DVD-1511.iso /var/www/html/
[root@bigdata-server-1 ~]# mkdir /var/www/html/os
[root@bigdata-server-1 ~]# tar -zxvf /var/www/html/hdp.tar.gz
[root@bigdata-server-1 ~]# mount -o loop -t iso9660 /var/www/html/CentOS-7-x86_64-DVD-1511.iso /var/www/html/os
mount: /dev/loop0 is write-protected, mounting read-only
[root@bigdata-server-1 ~]# rm -fr /opt/os
```

> 设置开机自动挂载系统源

```
[root@bigdata-server-1 ~]# echo "mount -o loop -t iso9660 /var/www/html/CentOS-7-x86_64-DVD-1511.iso /var/www/html/os" >> /etc/rc.d/rc.local

[root@bigdata-server-1 ~]# cat /etc/rc.d/rc.local | grep mount
mount -o loop -t iso9660 /var/www/html/CentOS-7-x86_64-DVD-1511.iso /var/www/html/os

[root@bigdata-server-1 ~]# chmod 777 /etc/rc.d/rc.local
[root@bigdata-server-1 ~]# ll /etc/rc.d/rc.local

-rwxrwxrwx. 1 root root 558 Jul 27 16:03 /etc/rc.d/rc.local
```

### 修改repo文件

1. 配置HDP的repo源

> 配置

```
[root@bigdata-server-1 ~]# vim /etc/yum.repos.d/ambari.repo 

[ambari-2.6.1.0]
name=ambari Version - ambari-2.6.1.0
baseurl=http://192.168.0.81/hdp/ambari/centos7
gpgcheck=1
gpgkey=http://192.168.0.81/hdp/ambari/centos7/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
enabled=1
priority=1


[root@bigdata-server-1 ~]# yum clean all

Loaded plugins: fastestmirror
Cleaning repos: CentOS7 ambari-2.6.1.0
Cleaning up everything
Cleaning up list of fastest mirrors
```

> 验证

```
[root@bigdata-server-1 ~]# yum search ambari-server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
======================================= N/S matched: ambari-server =======================================
ambari-server.x86_64 : Ambari Server

  Name and summary matches only, use "search all" for everything.
```


2. 配置系统镜像源

> 配置

```
[root@bigdata-server-1 ~]# rm -fr /etc/yum.repos.d/CentOS7.repo

[root@bigdata-server-1 ~]# vim /etc/yum.repos.d/CentOS7.repo

[CentOS7] 
name=Centos7 Base
baseurl=http://192.168.0.81/os
enabled=1 
gpgcheck=0

[root@bigdata-server-1 ~]# yum clean all
Loaded plugins: fastestmirror
Cleaning repos: CentOS7
Cleaning up everything
Cleaning up list of fastest mirrors

```
> 验证


```
[root@bigdata-server-1 ~]# yum search vim

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
============================================ N/S matched: vim ============================================
vim-X11.x86_64 : The VIM version of the vi editor for the X Window System
vim-common.x86_64 : The common files needed by any version of the VIM editor
vim-enhanced.x86_64 : A version of the VIM editor which includes recent enhancements
vim-filesystem.x86_64 : VIM filesystem layout
vim-minimal.x86_64 : A minimal version of the VIM editor

  Name and summary matches only, use "search all" for everything.
```

### 关闭防火墙

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

### 验证

> 在浏览器中输入http://IP/os或者http://IP/hdp
> 能列出文件则搭建的内网离线源可用
