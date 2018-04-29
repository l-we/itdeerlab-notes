### 环境准备

 - CentOS7.2.1511操作系统
 - 系统语言为英文
 - 系统最小化安装
 - Linux内核版本3.10.0
 - 使用的用户：root
 - 登录密码为：123456
 - 主机名称为：demo

### 系统检查

```
[root@demo ~]# uname -a
Linux demo 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

[root@demo ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```

### 软件安装


[1] 字体管理工具

```
[root@demo ~]# yum install  fontconfig mkfontscale -y

Loaded plugins: fastestmirror
......

Installed:
  xorg-x11-font-utils.x86_64 1:7.5-20.el7                                                  

Dependency Installed:
  libXfont.x86_64 0:1.5.2-1.el7               libfontenc.x86_64 0:1.1.3-3.el7              

Complete!
```

[2] 准备Window字体

> 打开Windows操作系统的C盘 --> 打开[Windows] --> 打开[Fonts] --> 找到[微软雅黑] 复制出来，放在一个文件夹下，压缩上传到Linux操作系统下的/tmp目录下


[3] 解压

```
[root@demo ~]# cd /tmp/

[root@demo tmp]# unzip font.zip 
Archive:  font.zip
   creating: font/
  inflating: font/msyh.ttf           
  inflating: font/msyhbd.ttf 
```

[4] 设置字体


```
[root@demo tmp]# mkdir -p /usr/share/fonts/win
[root@demo tmp]# mv /tmp/font/* /usr/share/fonts/win/
[root@demo tmp]# ll /usr/share/fonts/win/
total 35524
-rw-r--r--. 1 root root 14602860 Jun 11  2009 msyhbd.ttf
-rw-r--r--. 1 root root 21767952 Jun 11  2009 msyh.ttf

```


[5] 更新字体缓存

```
[root@demo tmp]# cd /usr/share/fonts/win/
[root@demo win]# mkfontscale
[root@demo win]# mkfontdir
[root@demo win]# fc-cache
```

[6] 重启机器

```
[root@demo win]# reboot
```

> 添加完成