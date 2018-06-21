
### Window下安装Maven

> Maven：百度百科上这样说 Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。 Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，许多 Apache Jakarta 项目发文时使用 Maven，而且公司项目采用 Maven 的比例在持续增长。

> 我对它的作用认为就是一个非常好用的项目管理工具

### Window下安装

[1] 安装JDK

> 这里不赘述，详情，请参考[JDK在Window下安装配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/JDK/UserGuide/JDK%E5%9C%A8Window%E4%B8%8B%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE.md)

[2] 安装包下载

> 下载地址：https://maven.apache.org/download.cgi

 - 找到下载页面

 ![201862118121](http://panrhkqz9.bkt.clouddn.com/201862118121.png)

> 这里下载3.5.3的版本

```
apache-maven-3.5.3-bin.tar.gz
```

> 放到D盘的根目录下,解压，改名为Maven

![2018621181619](http://panrhkqz9.bkt.clouddn.com/2018621181619.png)

![2018621182946](http://panrhkqz9.bkt.clouddn.com/2018621182946.png)

[3] 配置环境变量

 - 在电脑桌面[我的电脑] 点击右键，[属性]

![2018621181816](http://panrhkqz9.bkt.clouddn.com/2018621181816.png)

![2018621181844](http://panrhkqz9.bkt.clouddn.com/2018621181844.png)

![2018621181933](http://panrhkqz9.bkt.clouddn.com/2018621181933.png)

![2018621182056](http://panrhkqz9.bkt.clouddn.com/2018621182056.png)

![2018621182121](http://panrhkqz9.bkt.clouddn.com/2018621182121.png)

![2018621182516](http://panrhkqz9.bkt.clouddn.com/2018621182516.png)

 - 接着返回去一步一步的确定

 - 使用组合键win + R 然后输入cmd 打开Docs窗口

 ![2018621182710](http://panrhkqz9.bkt.clouddn.com/2018621182710.png)

 - 使用mvn -v 命令验证

 ![2018621182825](http://panrhkqz9.bkt.clouddn.com/2018621182825.png)

> Maven安装成功，接下来可以使用Maven管理自己的项目了。
