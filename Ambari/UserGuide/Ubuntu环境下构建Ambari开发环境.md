# Ubuntu环境下构建Ambari开发编译环境

> 用过大数据平台的攻城狮们，应该对Ambari有所了解。
> Ambari是界面化构建大数据平台的开源软件。
> 下面介绍一下如何构建Ambari的开发环境

### 构建环境

```
系统: ubuntu-16.04
JDK：Oracle 1.7
Maven：apache 3.3.9
```

> 创建目录及安装软件

```
sudo mkdir -p /opt/install

cd /opt/install
sudo mkdir source

sudo apt-get install vim -y
sudo apt-get install wget -y
```

### ubuntu 安装Pycharm

```
wget https://download.jetbrains.8686c.com/python/pycharm-professional-2016.3.1.tar.gz
或者
https://www.jetbrains.com/pycharm/
DOWNLOAD NOW --> Professional Full-featured IDE for Python & Web development
```

> 下载到/opt/install目录下

```
sudo tar -zxvf pycharm-*.tar.gz
sudo mv pycharm-*.tar.gz source 
sudo mv pycharm-* pycharm
```

### ubuntu 配置编译环境

 - A.下载Oracle JDK1.7的压缩包
    
> 本机下载之后上传到服务器
> 把安装包上传至/opt/install
> 安装包：jdk-7u79-linux-x64.tar.gz
	
```
sudo tar -zxvf jdk-*.tar.gz
sudo mv jdk-*.tar.gz source
sudo mv jdk* jdk
```

> 配置环境变量

```
vim /etc/profile.d/java.sh
	
export JAVA_HOME=/opt/install/jdk
export JRE_HOME=/opt/install/jdk/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

source /etc/profile

```

 - B.下载node

> wget https://nodejs.org/dist/v0.10.44/node-v0.10.44-linux-x64.tar.gz
> 或者：
> https://nodejs.org/dist/v0.10.44/  选择下载的包
	
```
sudo tar -zxvf node-*.tar.gz
sudo mv node-*.tar.gz source 
sudo mv node* node
```

> 配置环境变量

```
vim /etc/profile.d/node.sh

export NODE_HOME=/opt/install/node
export PATH=$NODE_HOME/bin:$PATH

source /etc/profile
```

 - C.下载maven的安装包

> 本机下载之后上传到服务器
> 上传到/opt/install
> 安装包：apache-maven-3.3.9-bin.tar.gz
	
```
sudo tar -zxvf apache-maven*.tar.gz
sudo mv apache-maven*.tar.gz source
sudo mv apache-maven* maven
```

> 配置环境变量

```
vim /etc/profile.d/maven.sh

export MAVEN_HOME=/opt/install/maven
export PATH=$MAVEN_HOME/bin:$PATH

source /etc/profile
```

### ubuntu 配置python

```
development@ubuntu:~$ python
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 

退出（Ctrl + z）

sudo apt-get install python-dev
```

### ubuntu 下载ambari源码

> 创建工作目录

```
mkdir -p /opt/work
cd /opt/work

wget https://github.com/apache/ambari/archive/release-2.2.2.tar.gz
或者打开
https://github.com/apache/ambari/releases/ 下载需要的版本
sudo tar -zxvf release*.tar.gz
```

### ubuntu 配置brunch

> 在当前这个普通用户上，我尝试了好几次在安装brunch的时候安装不上
> 如果是刚刚安装好的Ubuntu系统还没有登录到root这里需要给root设置密码

```
sudo su - root
输入当前用户密码

sudo passwd root
输入root的密码
确认root的密码
```

> 安装brunch

```
npm install -g brunch@1.7.20
等待安装成功
cd /opt/work/ambari解压目录/ambari-web

执行
npm install
```

> 验证环境变量

```
逐个验证配置的环境变量是否生效
source /etc/profile

java -version
mvn -v 
npm -v
node -v
```

### ubuntu brunch调试ambari

```
brunch w -s
27 Dec 19:10:34 - info: application started on http://localhost:3333/
```

### ubuntu pycharm导入ambari代码

```
sh /opt/cloud/pycharm/bin/pycharm.sh
这里就不详细介绍了
就是打开pycharm编辑器，输入注册码（网上能找到）
跳过一些配置，就进入了pycharm
点击open，找到ambari的目录，就下一步下一步的完成就行了。

打开源代码进行修改，编辑，保存
```

### ubuntu 编译amabri

> 回到amabri 的根目录
> 编译要依赖一些环境
	
```
rpm-buildrpm-build
gcc-c++
rpmbuild
git
等
```

> 接下来开始编译

```
mvn versions:set -DnewVersion=2.2.2.0.0
pushd ambari-metrics
mvn versions:set -DnewVersion=2.2.2.0.0
popd
export MAVEN_OPTS="-Xmx2g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m"

mvn -B clean install package jdeb:jdeb -DnewVersion=2.2.2.0.0 -DskipTests -Dpython.ver="python >= 2.6" -Drat.numUnapprovedLicenses=100 -X -Preplaceurl    #for ubuntu

mvn -B install package rpm:rpm -DnewVersion=2.2.1 -DskipTests -Dpython.ver="python >= 2.6" -Preplaceurl #For redhat/centos
```
> 直到编译完成，中间可能时间较长，编译出错，找找错误信息，找到原因，解决掉。
> 多编译几次就过去了,编译过程出现的问题多数是因为环境的问题，会有很多的依赖。
> 很多时候会去国外下载Jar包，下载不下来出现问题居多。