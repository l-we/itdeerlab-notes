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

[root@work ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
[root@work ~]# uname -a
Linux work 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

### 编译环境准备

[1] 安装Maven

[2] 安装SBT

[root@work ~]# mkdir -p /opt/install
[root@work ~]# cd /opt/install/
[root@work install]# wget https://piccolo.link/sbt-1.1.4.zip
--2018-05-23 08:32:09--  https://piccolo.link/sbt-1.1.4.zip

Connecting to sbt-downloads.cdnedge.bluemix.net (sbt-downloads.cdnedge.bluemix.net)|23.42.188.250|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 45989766 (44M) [application/zip]
Saving to: ‘sbt-1.1.4.zip’

100%[===================================>] 45,989,766  1.69MB/s   in 82s    

2018-05-23 08:33:38 (545 KB/s) - ‘sbt-1.1.4.zip’ saved [45989766/45989766]

[root@work install]# ls
sbt-1.1.4.zip
[root@work install]# unzip sbt-1.1.4.zip 
Archive:  sbt-1.1.4.zip
  inflating: sbt/conf/sbtconfig.txt  
  inflating: sbt/conf/sbtopts        
  inflating: sbt/bin/sbt.bat         
  inflating: sbt/lib/local-preloaded/com.trueaccord.scalapb/scalapb-runtime_2.12/0.6.0/jars/scalapb-runtime_2.12.jar  
  inflating: sbt/lib/local-preloaded/com.trueaccord.scalapb/scalapb-runtime_2.12/0.6.0/jars/scalapb-runtime_2.12.jar.sha1  
  inflating: sbt/lib/local-preloaded/com.trueaccord.scalapb/scalapb-runtime_2.12/0.6.0/jars/scalapb-runtime_2.12.jar.md5  
[root@work install]# ls
sbt  sbt-1.1.4.zip
[root@work install]# ls sbt
bin  conf  lib
[root@work install]# vim /etc/profile.d/sbt.sh


export SBT_HOME=/opt/install/sbt
export PATH=$SBT_HOME/bin:$PATH

### 源码准备


### 编译


### 验证



### 软件安装

[1] 清除yum的缓存

```
[root@study ~]# yum makecache fast
Loaded plugins: fastestmirror
base                                                                   | 3.6 kB  00:00:00     
extras                                                                 | 3.4 kB  00:00:00     
nginx                                                                  | 2.9 kB  00:00:00     
updates                                                                | 3.4 kB  00:00:00     
Loading mirror speeds from cached hostfile
 * base: mirror.bit.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirrors.tuna.tsinghua.edu.cn
Metadata Cache Created
```

[2] 列出Docker的版本

```
[root@study ~]# yum list docker --showduplicates
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.bit.edu.cn
 * extras: mirrors.tuna.tsinghua.edu.cn
 * updates: mirrors.tuna.tsinghua.edu.cn
Available Packages
docker.x86_64                     2:1.12.6-48.git0fdc778.el7.centos                     extras
docker.x86_64                     2:1.12.6-55.gitc4618fb.el7.centos                     extras
docker.x86_64                     2:1.12.6-61.git85d7426.el7.centos                     extras
docker.x86_64                     2:1.12.6-68.gitec8512b.el7.centos                     extras
```

[3] 安装指定版本的Docker

```
[root@study ~]# yum install docker-1.12.6-68.gitec8512b.el7.centos -y

......

Updated:
  dracut.x86_64 0:033-502.el7         selinux-policy-targeted.noarch 0:3.13.1-166.el7_4.7    
  systemd.x86_64 0:219-42.el7_4.4    

Dependency Updated:
  audit.x86_64 0:2.7.6-3.el7                    audit-libs.x86_64 0:2.7.6-3.el7               
  dracut-config-rescue.x86_64 0:033-502.el7     dracut-network.x86_64 0:033-502.el7           
  libgudev1.x86_64 0:219-42.el7_4.4             libselinux.x86_64 0:2.5-11.el7                
  libselinux-python.x86_64 0:2.5-11.el7         libselinux-utils.x86_64 0:2.5-11.el7          
  libsemanage.x86_64 0:2.5-8.el7                libsepol.x86_64 0:2.5-6.el7                   
  policycoreutils.x86_64 0:2.5-17.1.el7         selinux-policy.noarch 0:3.13.1-166.el7_4.7    
  systemd-libs.x86_64 0:219-42.el7_4.4          systemd-sysv.x86_64 0:219-42.el7_4.4          

Complete!
```

[4] 启动Docker

 - 首先要关闭Selinux

```
[root@study ~]# vim /etc/selinux/config

SELINUX=disabled
```

 - 启动

```
[root@study ~]# systemctl start docker
```

[5] 设置开机启动

```
[root@study ~]# systemctl enable docker

Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
```

### 结果验证

```
[root@study ~]# systemctl status  docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2017-12-26 19:30:57 CST; 11s ago
     Docs: http://docs.docker.com
 Main PID: 30037 (dockerd-current)
   CGroup: /system.slice/docker.service
           ├─30037 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/d...
           └─30043 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontaine...

Dec 26 19:30:56 study dockerd-current[30037]: time="2017-12-26T19:30:56.538624522+08:00" ...s"
Dec 26 19:30:56 study dockerd-current[30037]: time="2017-12-26T19:30:56.539971392+08:00" ...d"
Dec 26 19:30:56 study dockerd-current[30037]: time="2017-12-26T19:30:56.540503398+08:00" ...."
Dec 26 19:30:56 study dockerd-current[30037]: time="2017-12-26T19:30:56.568044745+08:00" ...e"
Dec 26 19:30:56 study dockerd-current[30037]: time="2017-12-26T19:30:56.817643211+08:00" ...s"
Dec 26 19:30:57 study dockerd-current[30037]: time="2017-12-26T19:30:56.997442143+08:00" ...."
Dec 26 19:30:57 study dockerd-current[30037]: time="2017-12-26T19:30:56.997762894+08:00" ...n"
Dec 26 19:30:57 study dockerd-current[30037]: time="2017-12-26T19:30:56.997795370+08:00" ....6
Dec 26 19:30:57 study dockerd-current[30037]: time="2017-12-26T19:30:57.026193934+08:00" ...k"
Dec 26 19:30:57 study systemd[1]: Started Docker Application Container Engine.
Hint: Some lines were ellipsized, use -l to show in full.

或者

[root@study ~]# docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 1.12.6
Storage Driver: devicemapper
 Pool Name: docker-253:0-292564-pool
 Pool Blocksize: 65.54 kB
 Base Device Size: 10.74 GB
 Backing Filesystem: xfs
 Data file: /dev/loop0
 Metadata file: /dev/loop1
 Data Space Used: 11.8 MB
 Data Space Total: 107.4 GB
 Data Space Available: 107.4 GB
 Metadata Space Used: 581.6 kB
 Metadata Space Total: 2.147 GB
 Metadata Space Available: 2.147 GB
 Thin Pool Minimum Free Space: 10.74 GB
 Udev Sync Supported: true
 Deferred Removal Enabled: true
 Deferred Deletion Enabled: true
 Deferred Deleted Device Count: 0
 Data loop file: /var/lib/docker/devicemapper/devicemapper/data
 WARNING: Usage of loopback devices is strongly discouraged for production use. Use `--storage-opt dm.thinpooldev` to specify a custom block storage device.
 Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
 Library Version: 1.02.107-RHEL7 (2015-10-14)
Logging Driver: journald
Cgroup Driver: systemd
Plugins:
 Volume: local
 Network: null host bridge overlay
Swarm: inactive
Runtimes: docker-runc runc
Default Runtime: docker-runc
Security Options: seccomp selinux
Kernel Version: 3.10.0-327.el7.x86_64
Operating System: CentOS Linux 7 (Core)
OSType: linux
Architecture: x86_64
Number of Docker Hooks: 3
CPUs: 2
Total Memory: 7.625 GiB
Name: study
ID: 3GNZ:64PX:NQC3:I2RM:ZUWS:NMRN:BCXF:5T5K:XQMO:YPFU:343A:4AQU
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
Insecure Registries:
 127.0.0.0/8
Registries: docker.io (secure)
```

> Docker 已经安装完成，可以使用了