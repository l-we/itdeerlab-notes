
# teamview远程控制软件在Ubuntu环境下安装配置

> 以下介绍一个远程控制计算机软件，也是很稳定的一款软件，平时远程桌面时，用的也是非常的广泛。
> 这款远程软件有免费和商业版，平时自己使用选择免费版，完全没有问题。

### 安装环境：

> 系统：Ubuntu 16.04 64位
  软件：Teamview 12

### 下载软件

- A、 [打开TeamView的下载页面](https://www.teamviewer.com/en/download/linux/)

- B、找到 Ubuntu, Debian -> 点击 X86 64bit

- C、下载之后，上传到Ubuntu系统上去查看：teamviewer_12.0.76279_i386.deb


### 安装依赖

> 64位Ubuntu 16.04系统需要添加32位架构支持

```
sudo dpkg --add-architecture i386

sudo apt-get update
```

> 安装TeamViewer的依赖包。

```
sudo apt-get install libdbus-1-3:i386 libasound2:i386 libexpat1:i386 libfontconfig1:i386 libfreetype6:i386 libjpeg62:i386 libpng12-0:i386 libsm6:i386 libxdamage1:i386 libxext6:i386 libxfixes3:i386 libxinerama1:i386 libxrandr2:i386 libxrender1:i386 libxtst6:i386 zlib1g:i386 libc6:i386
```

### 安装TeamView

> 安装TeamView

```
首先进入到包存放的地址
sudo dpkg -i teamviewer*.deb
```

> 安装成功之后

```
teamviewer

可以启动

或者在Dash菜单里搜索TeamView启动

首先选择[接受协议]
```

> 需要控制对方的计算机，需要知道对方的ID和密码，让对让控制自己的计算机，把ID和密码（可以告诉随机生成的临密码）告诉对方，这样就能完成控制需要。

### 使用TeamView

> 设定固定的密码

> 点击软件视图左上方 点击 “Connection” 选项

> 设置无人值守 点击 “Set unattended access...”

> Set unattended access 直接 点击 “Next” 下一步

> 设置主机名称和连接密码填写完成， 点击 “Next” 下一步
 - Computer name:电脑名称（随意起名）
 - Password:密码（自己设置）
 - Confirm password:（密码确认）

> 选择第三项不创建Teamview账号 点击 “Next” 下一步
 - 选择“I don`t want to create a TeamViewer account”选项

> 这里就设置完成 点击“Finish” 完成

> 配置好无人值守，就可以远程使用自己设置的密码连接了
> 现在就可以远程别人计算机或者别人远程这台计算机了