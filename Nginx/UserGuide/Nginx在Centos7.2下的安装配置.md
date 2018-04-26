### 简要说明

> 这里介绍一下Linux下安装Nginx，平时总是听说Nginx多么牛，但是好像自己从来没有仔细研究过。这里我刚好有这样一个机会，把整个操作过程记录一下，供大家参考学习，如有不对之处，望不吝赐教。

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

###  Nginx两种安装方式

 - rpm包安装
 - 源码安装

[1] rpm包安装
    
> Centos7系统库中默认没有Nginx的rpm包，如果想使用rpm安装方式首先要更新rpm依赖库
    
 1. 安装Nginx相关库

```
[root@study ~]# rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

Retrieving http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
warning: /var/tmp/rpm-tmp.ACOW2J: Header V4 RSA/SHA1 Signature, key ID 7bd9bf62: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:nginx-release-centos-7-0.el7.ngx ################################# [100%]
```

 2. 安装Nginx

```
[root@study ~]# yum install nginx -y

.....

  Verifying  : 1:openssl-libs-1.0.2k-8.el7.x86_64                                                            2/5 
  Verifying  : 1:openssl-1.0.2k-8.el7.x86_64                                                                 3/5 
  Verifying  : 1:openssl-1.0.1e-42.el7.9.x86_64                                                              4/5 
  Verifying  : 1:openssl-libs-1.0.1e-42.el7.9.x86_64                                                         5/5 

Installed:
  nginx.x86_64 1:1.12.2-1.el7_4.ngx                                                                              
Dependency Updated:
  openssl.x86_64 1:1.0.2k-8.el7                        openssl-libs.x86_64 1:1.0.2k-8.el7                       
Complete!
```

 3. 启动Nginx

```
[root@study ~]# systemctl start nginx
```

 4. 检测Nginx是否启动成功
 
```
[root@study ~]# systemctl status nginx

● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Tue 2017-12-26 14:35:23 CST; 13s ago
     Docs: http://nginx.org/en/docs/
  Process: 10693 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
  Process: 10691 ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
 Main PID: 10695 (nginx)
   CGroup: /system.slice/nginx.service
           ├─10695 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─10696 nginx: worker process

Dec 26 14:35:23 study systemd[1]: Starting nginx - high performance web server...
Dec 26 14:35:23 study nginx[10691]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Dec 26 14:35:23 study nginx[10691]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Dec 26 14:35:23 study systemd[1]: Started nginx - high performance web server.
```

[2] 源码安装

 1. 安装依赖软件
 
```
[root@study ~]# yum install -y openssl openssl-devel gcc-c++ pcre pcre-devel zlib zlib-devel 

[root@study ~]# yum install wget lsof -y

```

 2. 下载源码（这里使用1.12.2版本）
 
```
[root@study ~]# cd /usr/local/
[root@study ~]# wget http://nginx.org/download/nginx-1.12.2.tar.gz

[root@study ~]# tar -zxvf nginx-1.12.2.tar.gz

[root@study ~]# cd nginx-1.12.2
```

 3. 编译安装
 
```
[root@study ~]# ./configure
[root@study ~]# make && make install
```

 4. 启动Nginx

```
[root@study ~]# cd ../nginx
[root@study ~]# ./sbin/nginx
```
 5. 验证

```
[root@study ~]# lsof -i :80

COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   21774   root    6u  IPv4  41391      0t0  TCP *:http (LISTEN)
nginx   21775 nobody    6u  IPv4  41391      0t0  TCP *:http (LISTEN)
```

 6. 浏览器访问
 
```
关闭防火墙
[root@study ~]# systemctl stop firewalld

[root@study ~]# systemctl disable firewalld

[root@study ~]# systemctl status firewalld

```

### 结果验证


> 浏览器地址栏中输入：http://192.168.1.200/

```
Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
```

> 至此Nginx安装完成，一般使用Nginx都是为了做负载均衡或者做反向代理。我会在其他的文章中逐渐介绍，这里先介绍那么多。