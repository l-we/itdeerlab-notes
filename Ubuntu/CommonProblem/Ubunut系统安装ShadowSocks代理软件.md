
# ShadowSocks代理在Ubuntu环境下安装设置

> 在Window操作系统上使用shadowsocks做代理是很方便的，但是开发环境迁移到Linux操作系统上之后，使用代理就要费一番周折了。在Linux环境中使用Google是非常方便的

### 安装环境：

> Ubuntu16.04 64位

### 安装shadowsocks服务器

- 更新软件

```
sudo apt-get update
```

- 安装pip

```
sudo apt-get install python-pip -y
```

- 安装shadowsocks

```
sudo pip install shadowsocks
```

### 配置shadowsocks服务器

- 创建shadowsocks.json文件

```
vim /etc/shadowsocks.json

{
    "server":"my_server_ip",
    "server_port":8388,
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb"
}

注解：

server：代理服务器地址
8388：代理服务器提供服务端口
local_port：本机启动端口
password：连接密码
timeout：设置连接超时时间
method：连接加密方法 加密方式 跟服务器保持一致
```

### 启动shadowsocks服务器

- 启动shadowsocks

```
sslocal -c /etc/shadowsocks.json

# 开启后显示以下内容，代表开启成功：
# INFO loading libcrypto from libcrypto.so.1.0.0
# INFO starting local at 127.0.0.1:1080
```

### Chrome浏览器配置

> 使用命令开启chrome

```
# 将chrome关闭，在终端执行以下
google-chrome --proxy-server=socks5://127.0.0.1:1080
# 或
chromium-browser --proxy-server=socks5://127.0.0.1:1080
命令 会打开chrome

```
> 安装chrome的SwitchySharp插件

```
首先在Chrome网上应用店 --> SwitchySharp 搜索 --> + 添加至CHROME
```
![2018621141238](http://panrhkqz9.bkt.clouddn.com/2018621141238.png)

```
安装完成之后进行以下配置
```

![201862114131](http://panrhkqz9.bkt.clouddn.com/201862114131.png)

# 5、设置开机启动


```
# 打开图形化开机启动项管理界面
gnome-session-properties
# 添加(Add) -> 名称(name)和描述(comment)随便填，命令(Command)填写如下： 
sudo sslocal -c /etc/shadowsocks.json
# 搞定

或者

vim /etc/rc.local
在exit 0语句之前加上
sudo sslocal -c /etc/shadowsocks.json

```

> 至此，可以开始翻墙之旅了