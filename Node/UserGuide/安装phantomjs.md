### PhantomJS在Linux下的配置及加速

> PhantomJS是一个无界面的,可脚本编程的WebKit浏览器引擎。它原生支持多种web 标准：DOM 操作，CSS选择器，JSON，Canvas 以及SVGPhantomJS是一个无界面的,可脚本编程的WebKit浏览器引擎。它原生支持多种web 标准：DOM 操作，CSS选择器，JSON，Canvas 以及SVG。


### 测试平台环境

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.210（study）

### 系统检查

```
[root@master ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@master ~]# uname -a
Linux master 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装

[1] 下载

```
[root@study root]# cd /opt/install
[root@study install]# wget https://github.com/Medium/phantomjs/releases/download/v1.9.19/phantomjs-1.9.8-linux-x86_64.tar.bz2
```

[2] PhantomJS的解压

```
[root@study install]# ls 
jdk1.8  node  phantomjs-1.9.8-linux-x86_64.tar.bz2

[root@study install]# yum install bzip2 -y

[root@study install]# bzip2 -d phantomjs-1.9.8-linux-x86_64.tar.bz2
[root@study install]# ls
jdk1.8  node  phantomjs-1.9.8-linux-x86_64.tar
[root@study install]# tar -xvf phantomjs-1.9.8-linux-x86_64.tar
[root@study install]# rm -fr phantomjs-1.9.8-linux-x86_64.tar
[root@study install]# mv phantomjs-1.9.8-linux-x86_64/ phantom

```

[3] 配置PhantomJS

```
[root@study install]# vim /etc/profile.d/phantomjs.sh

export PHANTOM_HOME=/opt/install/phantom
export PATH=$PATH:$PHANTOM_HOME/bin

[root@study install]# source /etc/profile

```

[4] 安装PhantomJS依赖

```
[root@study install]# yum install fontconfig -y
```

### 验证

```
[root@study install]# phantomjs -v
1.9.8
```