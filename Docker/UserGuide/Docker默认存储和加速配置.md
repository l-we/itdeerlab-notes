### 简要说明

> Docker的一些镜像及容器虽然不是很大很占存储，但是多了也会占用很大的空间，这时候需要能把容器及镜像放在一块盘中是最后的选择。同时在国内下载镜像是非常慢的，配置一个加速器是有必要的。

### 环境准备

 - Centos7.2操作系统
 - 英文版系统
 - Linux内核版本3.10
 - 用户：root
 - 密码：123456
 - 主机名：study

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.200

> 前提环境 使用上节安装好的Docker服务，详见[Docker在Centos7.2上的安装启动](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Docker/UserGuide/Docker%E5%9C%A8Centos7.2%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%90%AF%E5%8A%A8.md)


### 系统检查

```
[root@study ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@study ~]# uname -a
Linux study 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
检查Device Mapper(Docker的存储驱动)

[root@study ~]# grep device-mapper /proc/devices
253 device-mapper

```

### 运行状态查看

```
[root@study ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2018-01-03 10:43:27 CST; 7h ago
     Docs: http://docs.docker.com
 Main PID: 7307 (dockerd-current)
   Memory: 47.0M
   CGroup: /system.slice/docker.service
......

[root@study ~]# docker -v
Docker version 1.12.6, build ec8512b/1.12.6
```

### 修改默认存储路径（还没有下载镜像时）

- 修改配置文件

```
[root@study ~]# vim /etc/sysconfig/docker

添加--graph=/data/docker指定存储路径为/data/docker
OPTIONS='--selinux-enabled --graph=/data/docker --log-driver=journald --signature-verification=false'
if [ -z "${DOCKER_CERT_PATH}" ]; then
    DOCKER_CERT_PATH=/etc/docker
fi
```

- 重启服务

```
[root@study ~]# systemctl restart docker
```

- 验证

```
[root@study ~]# ll /data/docker/
total 4
drwxrwxrwx. 24 root root 4096 Jan  3 12:07 containers
drwxrwxrwx.  5 root root   50 Dec 24 19:30 devicemapper
drwxrwxrwx.  3 root root   25 Dec 24 19:02 image
drwxrwxrwx.  3 root root   18 Dec 24 19:02 network
drwxrwxrwx.  2 root root    6 Dec 24 19:02 swarm
drwxrwxrwx.  2 root root    6 Jan  3 12:05 tmp
drwxrwxrwx.  2 root root    6 Dec 24 19:02 trust
drwxrwxrwx.  2 root root   24 Dec 24 19:02 volumes
```

### 修改默认存储路径（已经有很多镜像和容器时）

- 停止服务

```
[root@study ~]# systemctl stop docker

```

- 迁移容器及镜像

```
[root@study ~]# mkdir -p /data/docker

[root@study ~]# mv -i /var/lib/docker/* /data/docker
```

- 做软连

```
[root@study ~]# ln -s /data/docker /var/lib/docker
```

- 启动服务

```
[root@study ~]# systemctl start docker
```

### 添加加速器

- 修改配置

```
[root@study ~]# vim /etc/docker/daemon.json

添加以下信息
{
        "registry-mirrors": ["https://yt8vs5uq.mirror.aliyuncs.com"]
}
```

> 这里的加速地址是在阿里云上申请的个人独有加速地址，每个有都可以申请，前提是需要有阿里云账号

- 重启服务

```
[root@study ~]# systemctl restart docker
```

> Docker 存储路径配置完成，加速配置完成。