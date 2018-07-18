
> 这里讲解一些Docker的简单使用

### Docker下载一个指定版本的CentOS系统

```
镜像源地址：https://hub.docker.com/explore/
```

```
docker pull centos:7.2.1511
```


### 容器导入和导出

```
导出：docker export 容器ID > my_container.tar
导入：cat mycontainer.tar | docker import - 镜像名称:标签
```

### 镜像的保存和加载

```
保存：docker save 镜像ID > my_image.tar 
		docker save itdeer.cn/centos-maven:3.3.9 > my_image.tar 
加载：docker load< my_image.tar

```


### 把容器保存成镜像

```
docker commit -m="描述" --author="作者" 容器的名称/ID 保存成镜像的名称：标签

其中 -m --author 是可以省略的

例如：

[root@work yum.repos.d]# docker commit -m="jdp-package 3.1.0.0 version compile environment" --author="itdeer.cn@gmail.com" b082558dc264 jdp-package/www.jikelab.com:centos-7.2
sha256:372bf63b94e4de39b0d05658f79d4759c15b75fc1af7132344dbfe62e259beeb
[root@work yum.repos.d]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
jdp-package/www.jikelab.com   centos-7.2          372bf63b94e4        10 seconds ago      3.28 GB
```

### 进入一个正在执行的容器

```
docker exec -it 容器名称/ID /bin/bash OR /bin/sh
```


### 构建JDK的Docker镜像：

```
构建的目录是：/home/sundafei/WorkSpace/Docker

docker run --rm -it fa2dc0a7f0a9 /bin/bash

docker build -t itdeer.cn/centos-jdk:1.8 .

docker build -t itdeer.cn/centos-maven:3.3.9 .

/opt/install/maven/working

```

### 编译过程跳过测试

```
mvn install -DskipTests
mvn install -Dmaven.test.skip=true
```


### 使用非ROOT用户使用Docker

```
以下操作在一个需要执行Docker的用户下进行
sudo groupadd docker
sudo gpasswd -a ${USER} docker
sudo service docker restart 
	OR
sudo gpasswd -a ${USER} docker

然后退出当前系统重新登录
su root
su demo 
docker ps
```