# Ubuntu系统开启远程终端

> Ubuntu桌面系统在安装完成之后是不能使用远程终端连接上的。
> 这时候在操作时就很不方便，上传文件啊，复制一些命令啊等等。
> 以下就介绍一下怎样开启远程终端连接。

### 环境：

> Ubuntu16.04 64位

### 首先在服务器上安装ssh的服务器端

```
sudo apt-get install openssh-client -y
sudo apt-get install openssh-server -y
```

> 很多时候会遇到一些问题 如：

![image](https://github.com/ItdeerLab/itdeerlab-notes/blob/images/Ubuntu/2018.06.13-1.png)

> 解决方式：

```
sudo rm -fr /var/chche/apt/archives/lock
sudo rm -fr /var/lib/dpkg/lock
```

### 启动ssh-server

```
sudo /etc/init.d/ssh start
```

> 确认ssh-server已经正常工作

```
netstat -tlp
```

### 设置开机启动ssh-server

```
vi /etc/init.d/rc.local

在exit 0语句前加上
/etc/init.d/ssh start
```

### 禁用防火墙

```
查看状态
sudo service ufw status  
停止防火墙
sudo service ufw stop
再次查看状态
sudo service ufw status
禁用防火墙
sudo ufw disable
```

### 配置密码登录

```
sudo vi /etc/ssh/sshd-config

PermitRootLogin 修改为yes

PubkeyAuthentication yes修改为no

AuthorizedKeysFile .ssh/authorized_keys前面加上#屏蔽掉，

PasswordAuthentication yes前面的#删除掉。
```

### 修改root密码

```
sudo root passwd 
连续两次输入密码即可
```

> 如果电脑是无线连接，在电脑上创建一个Ubuntu虚拟机，这时就要注意了，在Ubuntu桌面对网络要配置好，不然远程终端也是连接不上。（最好配置静态IP和DNS，选择好连接方式）
> 至此可以在远程终端连接到服务器了
