# DataiKu在Linux系统的安装配置

> DataiKu是法国一家公司研发的一款可视化数据分析工具，支持多种算法，同时可以使用Spark实现内存并行计算，提高计算速度。由于软件本身不是开源的，这里安装的是已经破解的版本。

### 测试平台环境（内核3.8及以上且64位操作系统，尽量英文版）

个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.0.248（compile）

### 查看机器信息
```
[root@compile ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```

> 涉及的文件有
- JDK (jdk-8u112-linux-x64.tar.gz)
- Config (configure.zip)
- 安装包 (dataiku-dss-3.1.5-new2.zip)
- Spark插件包 (sparkling-water-1.6.8.zip)


### 安装JDK

 - 下载安装包

```
mkdir -p /opt/install
cd /opt/install
wget http://192.168.0.220/source/jdk/jdk-8u112-linux-x64.tar.gz

注意： 我这里是自己搭建的源服务器，JDK的包从其他地方或者官网下载即可，或者本地上传也可以
```

 - 解压

```
tar -zxvf jdk-8u*.tar.gz
mv jdk1.8.0* jdk1.8
rm -fr jdk-8u*.tar.gz
```

 - 配置

```
vi /etc/profile.d/java.sh

export JAVA_HOME=/opt/install/jdk1.8
export JRE_HOME=/opt/install/jdk1.8/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

 - 使配置生效


```
source /etc/profile
```

 - 验证

```
java -version

java version "1.8.0_112"
Java(TM) SE Runtime Environment (build 1.8.0_112-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.112-b15, mixed mode)
```

### 创建用户

```
[root@compile ~]# useradd dataiku
[root@compile ~]# chmod 755 /etc/sudoers
[root@compile ~]# vim /etc/sudoers

root    ALL=(ALL)       ALL
dataiku ALL=(ALL)       ALL                   # 把dataiku用户加入sudo权限
# %wheel        ALL=(ALL)       NOPASSWD: ALL #把注释去掉，执行sudo命令不需要输入密码
[root@compile ~]# chmod 440 /etc/sudoers
[root@compile ~]# gpasswd -a dataiku wheel    # 把dataiku用户加入whell用户组
[root@compile root]# su dataiku
```

### 上传安装包

```
[dataiku@compile root]$ sudo mkdir /dataiku
[dataiku@compile root]$ sudo cd /dataiku
上传安装包至/dataiku文件目录下

[dataiku@compile dataiku]$ sudo unzip -n configure.zip -d configure
[dataiku@compile dataiku]$ sudo unzip dataiku-dss-3.1.5-new2.zip
[dataiku@compile dataiku]$ sudo ls
configure  configure.zip  dataiku-dss-3.1.5-new2  dataiku-dss-3.1.5-new2.zip
[dataiku@compile dataiku]$ sudo rm -fr dataiku-dss-3.1.5-new2.zip
[dataiku@compile dataiku]$ sudo rm -fr configure.zip
[dataiku@compile dataiku]$ ls
configure  dataiku-dss-3.1.5-new2
```

### 安装依赖

```
[root@compile dataiku]$ exit
[root@compile dataiku]$ cd

[root@compile ~]# yum install git libgfortran -y

[root@compile ~]# rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

Retrieving http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
warning: /var/tmp/rpm-tmp.54zsYv: Header V4 RSA/SHA1 Signature, key ID 7bd9bf62: NOKEY
Preparing...                          ################################# [100%]
        package nginx-release-centos-7-0.el7.ngx.noarch is already installed
        
[root@compile ~]# yum install nginx -y
......

[root@compile ~]# systemctl start nginx.service
[root@compile ~]# systemctl enable nginx.service
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```

### 安装配置

```
[root@compile ~]# su dataiku
[root@compile ~]# cd /dataiku

[root@compile dataiku]$ sudo mkdir install
[root@compile dataiku]$ sudo mv dataiku-dss-3.1.5-new2/dataiku-dss-3.1.5-new2/dataiku-dss-3.1.5/* install/

[root@compile dataiku]$ sudo cp /dataiku/configure/license.json install/

[root@compile dataiku]$ sudo chmod 777 /dataiku/install/installer.sh
[root@compile dataiku]$ sudo chmod 777 /dataiku/install/license.json
[root@compile dataiku]$ sudo chmod 777 /dataiku/install/dss-version.json

[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/tools/bin/*
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/*.sh
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/*.py
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/dataiku.boot.template
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/diagnosis

[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/*/*.sh
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/*/*.py
[root@compile dataiku]$ sudo chmod 777 -R /dataiku/install/scripts/linked/*
```

### 解决网络问题

```
[dataiku@compile dataiku]$ sudo -i "/dataiku/install/scripts/install/install-deps.sh"

......

Updated:
  expat.x86_64 0:2.1.0-10.el7_3           freetype.x86_64 0:2.4.11-15.el7           libgomp.x86_64 0:4.8.5-16.el7_4.1          

Dependency Updated:
  chkconfig.x86_64 0:1.7.4-1.el7          glib2.x86_64 0:2.50.3-3.el7            nspr.x86_64 0:4.13.1-1.0.el7_3               
  nss.x86_64 0:3.28.4-15.el7_4            nss-softokn.x86_64 0:3.28.3-8.el7_4    nss-softokn-freebl.x86_64 0:3.28.3-8.el7_4   
  nss-sysinit.x86_64 0:3.28.4-15.el7_4    nss-tools.x86_64 0:3.28.4-15.el7_4     nss-util.x86_64 0:3.28.4-3.el7               

Complete!
Note: Forwarding request to 'systemctl disable nginx.service'.
```

### 安装

```
[root@compile /]# chown dataiku:dataiku /dataiku/*
[root@compile /]# chown dataiku:dataiku /dataiku

[root@compile /]# su dataiku
[dataiku@compile /]$ cd /dataiku/install/
[dataiku@compile dataiku]$ ./installer.sh -d /dataiku/dataiku/dss_data -l /dataiku/install/license.json -p 11000

[dataiku@compile dataiku]$ cp /dataiku/configure/dip.properties /dataiku/configure/general-settings.json /dataiku/configure/user-settings.json /dataiku/configure/users.json /dataiku/configure/variables.json /dataiku/dataiku/dss_data/config/

[dataiku@compile dataiku]$ ll /dataiku/dataiku/dss_data/config/

total 48
-rw-rw-r--. 1 dataiku dataiku  697 Jan 10 19:24 connections.json
-rw-rw-r--. 1 dataiku dataiku   25 Jan 10 19:32 dip.properties
-rw-rw-r--. 1 dataiku dataiku 2424 Jan 10 19:32 general-settings.json
drwxrwxr-x. 2 dataiku dataiku    6 Jan 10 19:24 ipython_notebooks
-rw-r--r--. 1 dataiku dataiku  947 Jan 10 18:45 license.json
-rw-rw-r--. 1 dataiku dataiku  142 Jan 10 19:24 messaging-channels.json
-rw-r--r--. 1 dataiku dataiku    0 Jan 10 19:25 pnotifications.db
-rw-r--r--. 1 dataiku dataiku  512 Jan 10 19:25 pnotifications.db-journal
drwxrwxr-x. 2 dataiku dataiku    6 Jan 10 19:24 projects
-rw-rw-r--. 1 dataiku dataiku    2 Jan 10 19:24 public-apikeys.json
-rw-rw-r--. 1 dataiku dataiku   25 Jan 10 19:24 scheduler_status.json
-rw-r--r--. 1 dataiku dataiku    0 Jan 10 19:25 timelines.db
-rw-r--r--. 1 dataiku dataiku  512 Jan 10 19:25 timelines.db-journal
-rw-r--r--. 1 dataiku dataiku 1149 Jan 10 19:32 user-settings.json
-rw-rw-r--. 1 dataiku dataiku 2813 Jan 10 19:32 users.json
-rw-r--r--. 1 dataiku dataiku    2 Jan 10 19:32 variables.json
```

### 安装其他的类库

- 安装Spark环境

> 前提是已经安装好Spark并且有Spark的环境变量

```
[dataiku@compile dataiku]$ /dataiku/dataiku/dss_data/
[dataiku@compile dataiku]$ ./bin/dssadmin install-spark-integration

+ Using SPARK_HOME=/usr/hdp/current/spark-client
+ Found Spark version 1.6.3
+ Creating configuration file: /dataiku/dataiku/dataiku/dss_data/bin/env-spark.sh
+ Installing toree/jupyter integration
+ Enabling DSS Spark support
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=120m; support was removed in 8.0
[2018/01/10-21:27:54.693] [main] [INFO] [dku.logging]  - Loading logging settings
[2018/01/10-21:27:54.739] [main] [INFO] [dku]  - starting up ...
[2018/01/10-21:27:55.405] [main] [INFO] [org.springframework.beans.factory.support.DefaultListableBeanFactory]  - Overriding bean definition for bean 'dataService': replacing [Generic bean: class [com.dataiku.dip.shaker.server.DataService]; scope=singleton; abstract=false; lazyInit=false; autowireMode=0; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=null; initMethodName=null; destroyMethodName=null] with [Generic bean: class [com.dataiku.dip.shaker.server.DataService]; scope=singleton; abstract=false; lazyInit=false; autowireMode=0; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=null; initMethodName=null; destroyMethodName=null]
[2018/01/10-21:27:55.742] [main] [INFO] [org.springframework.beans.factory.support.DefaultListableBeanFactory]  - Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@16a0ee18: defining beans [org.springframework.context.annotation.internalConfigurationAnnotationProcessor,org.springframework.context.annotation.internalAutowiredAnnotationProcessor,org.springframework.context.annotation.internalRequiredAnnotationProcessor,org.springframework.context.annotation.internalCommonAnnotationProcessor,DAOProvider,springUtils,h2BasedTimestampsDAO,datasetStatusService,schedulerDAO,dataService,futureService,noopFutureKernelsManager,achievementService,notificationService,DKUTransactionService,generalSettingsDAO,DKULicenseStatusService,datasetWritingService,backendVariablesService,scenarioRunContext,VStackRecipeBasicService,groupingRecipeBasicService,windowRecipeBasicService,visualRecipesBaseService,predictionRecipesBasicService,filesBasedSavedModelsDAO,filesBasedManagedFolderDAO,managedFolderBasicService,tableColoringService,flowGraphService,shakerStreamService,sharedBasicMeaningsService,filesBasedMeaningsDAO,sampleBuilder,pluginSettingsDAO,kernelGeneralSettingsService,remoteFlowObjectEventService,jobsDatabaseAccessService,statsDatabaseService,projectCommitModeService,analysisDataService,org.springframework.context.annotation.ConfigurationClassPostProcessor.importAwareProcessor,connectionsDAO,datasetsDAO,recipesDAO,scenariosDAO,sessionsDAO,messagingChannelsDAO,usersDAO,exportDAO]; root of factory hierarchy
[2018/01/10-21:27:56.131] [main] [INFO] [dku.timestamps.sql]  - Already at version 1
[2018/01/10-21:27:56.161] [main] [INFO] [dku.timestamps.sql]  - Transaction time 1515590876161
[2018/01/10-21:27:56.523] [main] [INFO] [dku.meanings.service]  - Loading list of meanings
[2018/01/10-21:27:56.667] [main] [INFO] [dku.db.internal]  - Creating new connection
[2018/01/10-21:27:56.824] [main] [WARN] [dku.spark]  - Spark is already enabled
[2018/01/10-21:27:56.824] [main] [WARN] [dku.spark]  - Execution configs already exist, not overriding
[2018/01/10-21:27:56.832] [main] [DEBUG] [dip.transactions]  - Begin commit
[2018/01/10-21:27:56.893] [main] [INFO] [dip.transactions]  - Candidate commit [GLOBAL] [DKU] CLI: Enabled Spark (from command-line) : 
 - /general-settings.json

[2018/01/10-21:27:56.895] [main] [DEBUG] [com.dataiku.dip.transactions.fs.FSyncUtils]  - writeAndSync: /dataiku/dataiku/dss_data/config/.ts/general-~settings.json
[2018/01/10-21:27:56.903] [main] [DEBUG] [com.dataiku.dip.transactions.fs.FSyncUtils]  - writeAndSync: /dataiku/dataiku/dss_data/config/.journal
[2018/01/10-21:27:56.918] [main] [DEBUG] [com.dataiku.dip.transactions.git.CommitQueue]  - Executing Git commit:
[GLOBAL] [DKU] CLI: Enabled Spark (from command-line) : 
 - /general-settings.json

[2018/01/10-21:27:57.052] [main] [INFO] [dip.transactions]  - Transaction committed
```

- 集成H2O环境

```
[dataiku@compile dss_data]$ vim /dataiku/install/scripts/_install-h2o-integration.sh

更改1.6.*）：
DOWNLOAD_URL="http://h2o-release.s3.amazonaws.com/sparkling-water/rel-1.6/8/sparkling-water-1.6.8.zip"
为：
 DOWNLOAD_URL="http://192.168.0.220/source/dataiku/sparkling-water-1.6.8.zip"


[dataiku@compile dataiku]$ ./dataiku/dss_data/bin/dssadmin install-h2o-integration

++ Downloading Sparkling Water
++ Downloading from: http://192.168.0.220/source/dataiku/sparkling-water-1.6.8.zip
Archive:  sparkling-water-1.6.8.zip
   creating: sparkling-water-1.6.8/
  inflating: sparkling-water-1.6.8/DEVEL.md  
   creating: sparkling-water-1.6.8/docker/
   creating: sparkling-water-1.6.8/docker/sparkling-water/
  inflating: sparkling-water-1.6.8/docker/sparkling-water/build.sh  
   creating: sparkling-water-1.6.8/docker/sparkling-water/base/
  inflating: sparkling-water-1.6.8/docker/sparkling-water/base/Dockerfile  
 extracting: sparkling-water-1.6.8/docker/sparkling-water/base/README.md  
  inflating: sparkling-water-1.6.8/docker/build.sh  
  inflating: sparkling-water-1.6.8/docker/README.md  
   creating: sparkling-water-1.6.8/docker/bin/
  inflating: sparkling-water-1.6.8/docker/bin/sparkling-shell  
  inflating: sparkling-water-1.6.8/docker/bin/run-example.sh  
  inflating: sparkling-water-1.6.8/docker/bin/common.sh  
  inflating: sparkling-water-1.6.8/docker/build.gradle  
   creating: sparkling-water-1.6.8/assembly/
   creating: sparkling-water-1.6.8/assembly/build/
   creating: sparkling-water-1.6.8/assembly/build/libs/
  inflating: sparkling-water-1.6.8/assembly/build/libs/sparkling-water-assembly-1.6.8-all.jar  
  inflating: sparkling-water-1.6.8/CHANGELOG.md  
  inflating: sparkling-water-1.6.8/README.md  
   creating: sparkling-water-1.6.8/bin/
  inflating: sparkling-water-1.6.8/bin/run-python-script.sh  
  inflating: sparkling-water-1.6.8/bin/pysparkling  
  inflating: sparkling-water-1.6.8/bin/sparkling-shell.cmd  
  inflating: sparkling-water-1.6.8/bin/run-example.cmd  
  inflating: sparkling-water-1.6.8/bin/sparkling-env.sh  
  inflating: sparkling-water-1.6.8/bin/sparkling-env.cmd  
  inflating: sparkling-water-1.6.8/bin/sparkling-shell2.cmd  
  inflating: sparkling-water-1.6.8/bin/sparkling-shell  
  inflating: sparkling-water-1.6.8/bin/run-example.sh  
  inflating: sparkling-water-1.6.8/bin/launch-spark-cloud.sh  
  inflating: sparkling-water-1.6.8/bin/run-sparkling.sh  
  inflating: sparkling-water-1.6.8/bin/run-example2.cmd  
  inflating: sparkling-water-1.6.8/gradle.properties  
  inflating: sparkling-water-1.6.8/LICENSE  
   creating: sparkling-water-1.6.8/examples/
   creating: sparkling-water-1.6.8/examples/scripts/
  inflating: sparkling-water-1.6.8/examples/scripts/chicagoCrimeSmallShell.script.scala  
  inflating: sparkling-water-1.6.8/examples/scripts/StrataAirlines.script.scala  
  inflating: sparkling-water-1.6.8/examples/scripts/chicagoCrimeSmall.script.scala  
  inflating: sparkling-water-1.6.8/examples/scripts/craigslistJobTitles.script.scala  
  inflating: sparkling-water-1.6.8/examples/scripts/hamOrSpam.script.scala  
  inflating: sparkling-water-1.6.8/examples/README.md  
   creating: sparkling-water-1.6.8/examples/flows/
  inflating: sparkling-water-1.6.8/examples/flows/Spark_SVM_Model.flow  
  inflating: sparkling-water-1.6.8/examples/flows/Strata_SJ_2016.flow  
  inflating: sparkling-water-1.6.8/examples/flows/2016_H2O_Tour_Chicago.flow  
  inflating: sparkling-water-1.6.8/examples/flows/Lending_Club.flow  
  inflating: sparkling-water-1.6.8/examples/flows/index.list  
  inflating: sparkling-water-1.6.8/examples/flows/Amazon_Fine_Food_Sentiment_Analysis.flow  
   creating: sparkling-water-1.6.8/examples/smalldata/
  inflating: sparkling-water-1.6.8/examples/smalldata/prostate.csv  
  inflating: sparkling-water-1.6.8/examples/smalldata/Chicago_Ohare_International_Airport.csv  
  inflating: sparkling-water-1.6.8/examples/smalldata/chicagoAllWeather.csv  
  inflating: sparkling-water-1.6.8/examples/smalldata/chicagoCrimes10k.csv  
  inflating: sparkling-water-1.6.8/examples/smalldata/allyears2k_headers.csv.gz  
  inflating: sparkling-water-1.6.8/examples/smalldata/smsData.txt  
  inflating: sparkling-water-1.6.8/examples/smalldata/craigslistJobTitles.csv  
  inflating: sparkling-water-1.6.8/examples/smalldata/year2005.csv.gz  
  inflating: sparkling-water-1.6.8/examples/smalldata/chicagoCensus.csv  
   creating: sparkling-water-1.6.8/py/
   creating: sparkling-water-1.6.8/py/pysparkling/
  inflating: sparkling-water-1.6.8/py/pysparkling/initializer.py  
  inflating: sparkling-water-1.6.8/py/pysparkling/context.py  
  inflating: sparkling-water-1.6.8/py/pysparkling/__init__.py  
  inflating: sparkling-water-1.6.8/py/pysparkling/conf.py  
  inflating: sparkling-water-1.6.8/py/pysparkling/conversions.py  
   creating: sparkling-water-1.6.8/py/examples/
   creating: sparkling-water-1.6.8/py/examples/scripts/
  inflating: sparkling-water-1.6.8/py/examples/scripts/ChicagoCrimeDemo.py  
  inflating: sparkling-water-1.6.8/py/examples/scripts/H2OContextInitDemo.py  
   creating: sparkling-water-1.6.8/py/examples/notebooks/
  inflating: sparkling-water-1.6.8/py/examples/notebooks/TensorFlowDeepLearning.ipynb  
  inflating: sparkling-water-1.6.8/py/examples/notebooks/ChicagoCrimeDemo.ipynb  
  inflating: sparkling-water-1.6.8/py/examples/notebooks/README.md  
  inflating: sparkling-water-1.6.8/py/examples/notebooks/TF_model_in_Flow.png  
  inflating: sparkling-water-1.6.8/py/examples/notebooks/getCloud.png  
   creating: sparkling-water-1.6.8/py/dist/
  inflating: sparkling-water-1.6.8/py/dist/h2o_pysparkling_1.6-1.6.8-py2.7.egg  
+ DSS H2O support is enabled
```

- 集成TensorFlow环境


```
[dataiku@compile dataiku]$ wget https://bootstrap.pypa.io/get-pip.py
[dataiku@compile dataiku]$ sudo python get-pip.py

Collecting pip
  Downloading pip-9.0.1-py2.py3-none-any.whl (1.3MB)
    100% |████████████████████████████████| 1.3MB 17kB/s 
Collecting setuptools
  Downloading setuptools-38.4.0-py2.py3-none-any.whl (489kB)
    100% |████████████████████████████████| 491kB 18kB/s 
Collecting wheel
  Downloading wheel-0.30.0-py2.py3-none-any.whl (49kB)
    100% |████████████████████████████████| 51kB 18kB/s 
Installing collected packages: pip, setuptools, wheel
Successfully installed pip-9.0.1 setuptools-38.4.0 wheel-0.30.0
[dataiku@compile dataiku]$ sudo pip -V
pip 9.0.1 from /usr/lib/python2.7/site-packages (python 2.7)


[dataiku@compile dataiku]$ ./dataiku/dss_data/bin/pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp27-none-linux_x86_64.whl

Collecting tensorflow==1.0.0 from https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp27-none-linux_x86_64.whl
  Downloading https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp27-none-linux_x86_64.whl (44.1MB)
    100% |████████████████████████████████| 44.1MB 22kB/s 
Requirement already up-to-date: wheel in ./dataiku/dss_data/pyenv/lib/python2.7/site-packages (from tensorflow==1.0.0)
Collecting protobuf>=3.1.0 (from tensorflow==1.0.0)
  Downloading protobuf-3.5.1-cp27-cp27mu-manylinux1_x86_64.whl (6.4MB)
    100% |████████████████████████████████| 6.4MB 15kB/s 
Collecting numpy>=1.11.0 (from tensorflow==1.0.0)
  Downloading numpy-1.14.0-cp27-cp27mu-manylinux1_x86_64.whl (16.9MB)
    100% |████████████████████████████████| 16.9MB 22kB/s 
Collecting six>=1.10.0 (from tensorflow==1.0.0)
  Downloading six-1.11.0-py2.py3-none-any.whl
Collecting mock>=2.0.0 (from tensorflow==1.0.0)
  Downloading mock-2.0.0-py2.py3-none-any.whl (56kB)
    100% |████████████████████████████████| 61kB 28kB/s 
Requirement already up-to-date: setuptools in ./dataiku/dss_data/pyenv/lib/python2.7/site-packages (from protobuf>=3.1.0->tensorflow==1.0.0)
Collecting pbr>=0.11 (from mock>=2.0.0->tensorflow==1.0.0)
  Downloading pbr-3.1.1-py2.py3-none-any.whl (99kB)
    100% |████████████████████████████████| 102kB 38kB/s 
Collecting funcsigs>=1; python_version < "3.3" (from mock>=2.0.0->tensorflow==1.0.0)
  Downloading funcsigs-1.0.2-py2.py3-none-any.whl
Installing collected packages: six, protobuf, numpy, pbr, funcsigs, mock, tensorflow
  Found existing installation: six 1.10.0
    Not uninstalling six at /dataiku/install/python.packages, outside environment /dataiku/dataiku/dss_data/pyenv
  Found existing installation: numpy 1.10.4
    Not uninstalling numpy at /dataiku/install/python.packages, outside environment /dataiku/dataiku/dss_data/pyenv
Successfully installed funcsigs-1.0.2 mock-2.0.0 numpy-1.14.0 pbr-3.1.1 protobuf-3.5.1 six-1.11.0 tensorflow-1.0.0
```

- 集成R语言开发环境


```
[dataiku@compile dataiku]$ sudo yum install epel-release -y
[dataiku@compile dataiku]$ sudo yum update -y
[dataiku@compile dataiku]$ sudo yum clean all

[dataiku@compile dataiku]$ sudo yum install libicu-devel -y
[dataiku@compile dataiku]$ sudo yum install libcurl-devel -y
[dataiku@compile dataiku]$ sudo yum install openssl-devel -y
[dataiku@compile dataiku]$ sudo yum install zeromq-devel -y
[dataiku@compile dataiku]$ sudo yum install R-core-devel -y


[dataiku@compile dataiku]$ ./dataiku/dss_data/bin/dssadmin install-R-integration


......
```




### 启动服务

```
[dataiku@compile dataiku]$ ./dataiku/dataiku/dss_data/bin/dss start
Waiting for DSS supervisor to start ...
backend                          STARTING  
ipython                          STARTING  
nginx                            STARTING  
DSS started, pid=11896
Waiting for DSS backend to start ....
```

### 访问界面

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/DataiKu/2018.05.20-1.png)

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/DataiKu/2018.05.20-2.png)

> 安装完成，可以在界面进行数据分析挖掘



### 安装R语言依赖包


```
执行： install.packages("corrplot") 问题： trying to use CRAN without setting a mirror

解决方式： install.packages("corrplot",repos = "http://cran.us.r-project.org") 或者 install.packages("corrplot",repos = "https://cloud.r-project.org")
```