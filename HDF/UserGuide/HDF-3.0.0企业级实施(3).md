## 安装Ambari-server之后

> Ambari-server 正常启动起来之后

> 安装MySQL JDBC驱动


```
yum install mysql-connector-java* -y

```

> Ambari-server设置MySQL驱动


```
sudo ambari-server setup --jdbc-db=mysql --jdbc-driver=/usr/share/java/mysql-connector-java.jar

```


> 安装MySQL57版的源


```
yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm -y

```

> 安装MySQL服务

```
yum install mysql-community-server -y

```


> 启动MySQL

```
systemctl start mysqld.service

```

> 查看初始化密码

```
grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |tail -1

```

> 修改初始化密码

```
mysqladmin -u root -p password "Redoop123$%^"

输入上一步查看到的初始化密码

```

> 验证


```
mysql -u root -p 
输入 Redoop123$%^ 改后的密码是否能进入

```


 ## 创建数据库
 

> 创建 registry和streamline的库及表


```
create database registry;
create database streamline;

CREATE USER 'registry'@'%' IDENTIFIED BY 'R12$%34qw';
CREATE USER 'streamline'@'%' IDENTIFIED BY 'R12$%34qw';


GRANT ALL PRIVILEGES ON registry.* TO 'registry'@'%' WITH GRANT OPTION ;
GRANT ALL PRIVILEGES ON streamline.* TO 'streamline'@'%' WITH GRANT OPTION ;

commit;

```

> 创建 druid和superset的库及表

```
CREATE DATABASE druid DEFAULT CHARACTER SET utf8;
CREATE DATABASE superset DEFAULT CHARACTER SET utf8;

CREATE USER 'druid'@'%' IDENTIFIED BY '9oNio)ex1ndL';
CREATE USER 'superset'@'%' IDENTIFIED BY '9oNio)ex1ndL';


GRANT ALL PRIVILEGES ON *.* TO 'druid'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'superset'@'%' WITH GRANT OPTION;

commit;

```

## 安装HDF-Ambari-Mpack的包



```
cd /temp
wget http://192.168.0.220/source/hdf/hdf/hdf-ambari-mpack-3.0.0.0-453.tar.gz

ambari-server install-mpack --mpack=/tmp/hdf-ambari-mpack-3.0.0.0-453.tar.gz --verbose

ambari-server restart

```


> 下一步打开浏览器访问http://IP:8080,数据用户名admin 密码admin进入管理界面进行安装HDF

> 