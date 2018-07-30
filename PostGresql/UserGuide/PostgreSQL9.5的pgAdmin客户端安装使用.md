### 前提环境

> 使用上节中已经安装配置好的PostgreSQL服务，详见[PostGreSQL9.5的简单配置](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/PostGresql/UserGuide/PostGreSQL9.5%E7%9A%84%E7%AE%80%E5%8D%95%E9%85%8D%E7%BD%AE.md) 这一步骤可以不做,若自己不需要使用界面化的连接工具,偏向使用终端的化.

### 软件下载

> pgAdmin当前最新版本是pgAdmin 4 V3.0版本[下载页面](https://www.pgadmin.org/download/pgadmin-4-windows/)

![2018621143025](http://note.itdeer.cn/2018621143025.png)

![2018621143042](http://note.itdeer.cn/2018621143042.png)

### 软件安装

[1] 环境为Window10操作系统

> 文件为：pgadmin4-3.0-x86.exe

[2] 双击安装

1. Welcome to the pgAdmin 4 Setup Wizard 页面 点击【Next】按钮

2. License Agreement 页面 选择【I accept the agreement】选项 点击【Next】按钮

3. Select Destination Location 页面 更改安装目录【D:\pgAdmin4\v3】 点击【Next】按钮

4. Select Start Menu Folder 页面 点击【Next】按钮

5. Ready to Install 点击【Install】按钮

6. 等一会之后 点击【Finish】按钮

[3] 浏览器会自动打开

> http://127.0.0.1:[port]/browser/

> 之后就能进入到pgAdmin的管理界面中

[4] 建立连接

1. 浏览器页面的最左侧，能看到 Browser下 【Servers】图标，鼠标点击【Servers】图标右键，选择【Create】下拉框选择【Server...】

2. 在Create - Server页面的【General】标签页填写基本信息：
     - Name ：          当前测试写 demo
     - Server group :   默认Servers
     - Background :     默认
     - Foreground :     默认
     - Connect? :       勾选
     - Comments ：      描述信息

3. 在Create - Server页面的【Connection】标签页填写基本信息：
     - Host name/address ：      192.168.1.220[PostgreSQL服务所在的主机IP或者域名]
     - Port :                    5432
     - Maintenance database :    demo[上节中的测试数据库]
     - Username :                demo[上节中的测试用户]
     - Password :                12345678[上节中的测试用户密码]
     - Save password? ：         勾选[是否要保存密码]
     - Role :                    默认不填
     - Service :                 默认不填
     - 点击【Save】
4. 显示界面

![201862114312](http://note.itdeer.cn/201862114312.png)


5. 要退出的话,在桌面的下方任务栏中找到象鼻图标,右键, 点击 [Shutdown server] 之后点击[Yes]按钮.

> 界面效果还是不错的，Dashboard会实时显示一些信息，可以界面化管理PostgrSQL服务了。以上安装过程我就不贴图了，文字描述应该清楚.因为贴图在访问的时候占带宽太大，很多时候网络不好就是加载不出来。所以，我以上写的是文字提示，应该是很好看懂的。
