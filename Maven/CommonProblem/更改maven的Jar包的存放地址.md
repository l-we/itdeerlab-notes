
### 更改Maven的Jar的存放地址

> 需要安装好Maven，并配置好环境变量

### Windows下更改

> 首先，安装Maven请参考 [Window下安装Maven](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Maven/UserGuide/Window%E4%B8%8B%E5%AE%89%E8%A3%85Maven.md)

> 进入Maven的安装目录（这里是D:\Maven）,在安装目录下创建working目录

> 进入conf目录下，修改settings.xml文件

![201862122424](http://note.itdeer.cn/201862122424.png)

> 添加完成保存即可，之后下载的Jar文件会分门别类的存放到 D:\Maven\working 目录下。方便自己查找

### Linux下更改

> 首先，安装Maven请参考 [Linux下安装Maven](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Maven/UserGuide/Linux%E4%B8%8B%E5%AE%89%E8%A3%85Maven.md)

> 进入Maven的安装目录（这里是/opt/maven）,在安装目录下创建working目录

```
[root@itdeer opt]# cd maven/
[root@itdeer maven]# mkdir working
```

> 修改conf/setting.xml文件

![201862122437](http://note.itdeer.cn/201862122437.png)

```
[root@itdeer maven]# cd conf/
[root@itdeer conf]# vim settings.xml

添加如下：
<localRepository>/opt/maven/working</localRepository>
```

> 保存退出即可，完成配置