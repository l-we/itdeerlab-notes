### 第一步准备环境

[1] 安装一个Linux操作系统或者Window系统

> 这个自行安装即可

[2] 在操作系统中安装Docker虚拟化环境(这里列出Centos上的安装使用,Window请自行准备)

 - CentoOS7.2安装Docker请参考 [Docker在Centos7.2上的安装启动](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Docker/UserGuide/Docker%E5%9C%A8Centos7.2%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%90%AF%E5%8A%A8.md)

 - 如果需要简单配置存储和加速的话请参考 [Docker默认存储和加速配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Docker/UserGuide/Docker%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E5%92%8C%E5%8A%A0%E9%80%9F%E9%85%8D%E7%BD%AE.md)

[3] 导入CRF编译环境的镜像(编译CRF的组件使用的是Docker镜像的方式)

 - 下载镜像

 ```
[root@sundafei ~]# wget http://192.168.0.220/dist/images/crf-bigtop.tar
 ```

  - 导入镜像

```
[root@sundafei ~]# docker load < crf-bigtop.tar 
9c2f1836d493: Loading layer 202.6 MB/202.6 MB
0556589ea49d: Loading layer 3.239 GB/3.239 GB
Loaded image: crf-bigtop/www.redoop.com:centos-7.2

```

 - 查看

```
[root@sundafei ~]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
crf-bigtop/www.redoop.com   centos-7.2          e6192e61d802        3 weeks ago         3.41 GB
```

### 第二步下载Bigtop的源代码

[1] Clone源代码

```
[root@sundafei ~]# git clone http://sundafei@192.168.0.236/redoop-crf/crf-bigtop.git
Cloning into 'crf-bigtop'...
Password for 'http://sundafei@192.168.0.236': 
remote: Counting objects: 3006, done.
remote: Compressing objects: 100% (1304/1304), done.
remote: Total 3006 (delta 1063), reused 2919 (delta 1044)
Receiving objects: 100% (3006/3006), 869.33 MiB | 18.98 MiB/s, done.
Resolving deltas: 100% (1063/1063), done.

```

[2] 切换分支

```
[root@sundafei ~]# cd crf-bigtop
[root@sundafei crf-bigtop]# git branch
* master
[root@sundafei crf-bigtop]# git checkout crf3-3.0.0.0
Branch crf3-3.0.0.0 set up to track remote branch crf3-3.0.0.0 from origin.
Switched to a new branch 'crf3-3.0.0.0'
[root@sundafei crf-bigtop]# git branch
* crf3-3.0.0.0
  master

```

### 第三步准备组件源码

[1] 组件源码这里已经放置好

```
http://192.168.0.220/dist/crf/3.0.0.0-512/
```

### 第四步组件编译

> 共享目录根据当前环境进行配置

```
docker run --rm -v /root/:/ws -v /home/sundafei/Soft/install/maven/working/:/opt/maven-3.3.9/working/ -v /home/sundafei/.npm/:/root/.npm/ -v /home/sundafei/.cache/:/root/.cache -v /home/sundafei/.gradle7/:/root/.gradle -v /home/sundafei/.sbt/:/root/.sbt/ -v /home/sundafei/.ivy2/:/root/.ivy2 -v /home/sundafei/.pip/:/root/.cache/pip/ -v /home/sundafei/tmp:/tmp/ --workdir /ws crf-bigtop/www.redoop.com:centos-7.2 bash -l -c " cd crf-bigtop ;  ./gradlew zookeeper-clean ;  ./gradlew zookeeper-rpm"
```

### 打包成功之后的rpm包会放置到

```
/root/crf-bigtop/output/zookeeper/noarch/目录下
```


