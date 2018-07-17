
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


### 构建JDK的Docker镜像：
	构建的目录是：/home/sundafei/WorkSpace/Docker

	docker run --rm -it fa2dc0a7f0a9 /bin/bash

	docker build -t itdeer.cn/centos-jdk:1.8 .

	docker build -t itdeer.cn/centos-maven:3.3.9 .

	/opt/install/maven/working

### 编译过程跳过测试

	mvn install -DskipTests
	mvn install -Dmaven.test.skip=true



### 使用非ROOT用户使用Docker

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
