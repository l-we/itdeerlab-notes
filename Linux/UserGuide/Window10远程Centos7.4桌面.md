### 简要说明

> 个人工作要求，公司员工配置的都是笔记本电脑，但是像有些大数据相关程序的研发测试，在Linux环境下较为方便，所以我个人申请公司给开了一台Centos7的虚拟机。我这边刚好有一个多余的显示器，这不嘛，直接接上，一边是Windows一边是Linux。这里介绍一下我配置远程连接Centos桌面记录。

### 环境准备

 - Centos7.4操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：itdeer

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：172.24.4.238

### 系统检查

```
[root@itdeer ~]# cat /etc/redhat-release 
CentOS Linux release 7.4.1708 (Core) 

[root@itdeer ~]# uname -a
Linux itdeer 3.10.0-862.3.2.el7.x86_64 #1 SMP Mon May 21 23:36:36 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装

[1] 安装EPEL源

```
[root@itdeer ~]# rpm -qa|grep epel

[root@itdeer ~]# yum install epel-release -y

Loaded plugins: fastestmirror, langpacks                        
......
Installed:
  epel-release.noarch 0:7-11                                                                                                                                             
Complete!
```

[2] 安装xrdp

```
[root@localhost ~]# yum install xrdp -y
Loaded plugins: fastestmirror, langpacks
epel/x86_64/metalink                                                                                                                            
.......                                                                                                                                     
Complete!
```

[3] 安装tigervnc-server

```
[root@localhost ~]# yum install tigervnc-server

Installed:
  tigervnc-server.x86_64 0:1.8.0-5.el7

Complete!
```

[4] 设置VNC密码

```
[root@localhost ~]# vncpasswd root
Password: 12345678
Verify: 12345678
Would you like to enter a view-only password (y/n)? y
Password: 12345678
Verify: 12345678
```

[5] 启动xrdp

 - 首先要关闭Selinux

```
[root@itdeer ~]# vim /etc/selinux/config

SELINUX=disabled

[root@itdeer ~]# setenforce 0
```

 - 关闭防火墙

```
[root@localhost ~]# systemctl stop firewalld

[root@localhost ~]# systemctl disable firewalld

Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
```

 - 启动及查看状态

```
[root@localhost ~]# systemctl start xrdp

[root@localhost ~]# systemctl enable xrdp

Created symlink from /etc/systemd/system/multi-user.target.wants/xrdp.service to /usr/lib/systemd/system/xrdp.service.

[root@localhost ~]# systemctl status xrdp
● xrdp.service - xrdp daemon
   Loaded: loaded (/usr/lib/systemd/system/xrdp.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2018-06-05 23:59:14 EDT; 11s ago
......
Jun 05 23:59:14 localhost.localdomain xrdp[11520]: (11520)(139829098580416)[INFO ] listening to port 3389 on 0.0.0.0
```

### Window连接

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/linux/2018.06.06-1.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/linux/2018.06.06-2.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/linux/2018.06.06-3.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/linux/2018.06.06-4.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/linux/2018.06.06-5.png)

> 已经可以远程连接到Centos桌面了，可以愉快的接着开发了。