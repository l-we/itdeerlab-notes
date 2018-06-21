### 使用Idea创建一个maven项目

> 首先需要安装Maven

 - Window下安装 请参考[Window下安装Maven](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Maven/UserGuide/Window%E4%B8%8B%E5%AE%89%E8%A3%85Maven.md)

 - Linux下安装 请参考[Linux下安装Maven](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Maven/UserGuide/Linux%E4%B8%8B%E5%AE%89%E8%A3%85Maven.md)

> Maven是项目管理使用最方便的（个人认为在Java这方面），Idea开发工具做Java开发是相当的好用。

> 简单介绍一下使用Idea怎么创建一个Maven项目，

 - 点击创建一个新的项目

![201862123281](http://panrhkqz9.bkt.clouddn.com/201862123281.png)

 - 选择一个Maven项目

![2018621232844](http://panrhkqz9.bkt.clouddn.com/2018621232844.png)

 - 填写GroupId ArtifactId Version信息

![2018621232923](http://panrhkqz9.bkt.clouddn.com/2018621232923.png)

 - 项目名称及存储路径

![2018621232946](http://panrhkqz9.bkt.clouddn.com/2018621232946.png)

 - 项目的结构及点击选择自动导入Maven变化

![2018621233112](http://panrhkqz9.bkt.clouddn.com/2018621233112.png)

- 主要的操作就在这个pom.xml文件中

![2018621233221](http://panrhkqz9.bkt.clouddn.com/2018621233221.png)

> 添加阿里的maven仓库加速

```
<repositories>
    <repository>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

> 添加项目属性设置

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
    <log4j.version>1.2.17</log4j.version>
</properties>
```

> 添加测试依赖

```
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.22</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.22</version>
    </dependency>
</dependencies>
```

> 基本环境已经可以了，以上的依赖及属性配置添加都是可以不需要的，Maven项目就可以直接开始写代码了，只是添加一些基本的依赖，方便之后的开发。