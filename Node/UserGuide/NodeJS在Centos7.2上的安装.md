
### NodeJS在Linux下的配置及加速

> NodeJS是前端的软件包的控制管理工具

### 环境准备

> 个人准备了一台Centos7的Linux服务器，X86 64位操作系统，最小化安装。静态IP：192.168.1.210（study）

### 系统检查

```
[root@master ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)

[root@master ~]# uname -a
Linux master 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

### 软件安装

[1] 下载

```
[root@study root]# cd /opt/install
[root@study install]# wget https://nodejs.org/dist/v8.9.4/node-v8.9.4-linux-x64.tar.xz
```

[2] node的解压

```
[root@study install]# ls 
jdk1.8  node-v8.9.4-linux-x64.tar.xz

[root@study install]# xz -d node-v8.9.4-linux-x64.tar.xz
[root@study install]# ls 
jdk1.8  node-v8.9.4-linux-x64.tar

[root@study install]# tar -xvf node-v8.9.4-linux-x64.tar

[root@study install]# ls
jdk1.8  node-v8.9.4-linux-x64  node-v8.9.4-linux-x64.tar

[root@study install]# mv node-v8.9.4-linux-x64 node
[root@study install]# rm -fr node-v8.9.4-linux-x64.tar 

```

[3] 配置node

```
[root@study install]# vim /etc/profile.d/node.sh

export NODE_HOME=/opt/install/node
export PATH=$PATH:$NODE_HOME/bin

[root@study install]# source /etc/profile

```

### 结果验证

```
[root@study install]# node -v
v8.9.4
[root@study install]# npm -v
5.6.0
```


### 使用淘宝NPM加速

```
[root@study node]# npm install -g cnpm --registry=https://registry.npm.taobao.org
/opt/install/node/bin/cnpm -> /opt/install/node/lib/node_modules/cnpm/bin/cnpm
+ cnpm@5.2.0
added 711 packages in 56.446s
```

> 验证

```
[root@study node]# cnpm
Usage: cnpm [option] <command>
Help: http://cnpmjs.org/help/cnpm

  Extend command
    web                            open cnpm web (ex.: cnpm web)
    check [ingoreupdate]           check project dependencies, add ignoreupdate will not check modules' latest version(ex.: cnpm check, cnpm check -i)
    doc [moduleName]               open document page (ex.: cnpm doc urllib)
    sync [moduleName]              sync module from source npm (ex.: cnpm sync urllib)
    user [username]                open user profile page (ex.: cnpm user fengmk2)

  npm command use --registry=https://registry.npm.taobao.org
    where <command> is one of:
    add-user, adduser, apihelp, author, bin, bugs, c, cache,
    completion, config, ddp, dedupe, deprecate, docs, edit,
    explore, faq, find, find-dupes, get, help, help-search,
    home, i, info, init, install, isntall, la, link, list, ll,
    ln, login, ls, outdated, owner, pack, prefix, prune,
    publish, r, rb, rebuild, remove, restart, rm, root,
    run-script, s, se, search, set, show, shrinkwrap, star,
    start, stop, submodule, tag, test, tst, un, uninstall,
    unlink, unpublish, unstar, up, update, v, version, view,
    whoami
      npm <cmd> -h     quick help on <cmd>
      npm -l           display full usage info
      npm faq          commonly asked questions
      npm help <term>  search for help on <term>
      npm help npm     involved overview

      Specify configs in the ini-formatted file:
          /root/.cnpmrc
      or on the command line via: npm <command> --key value
      Config info can be viewed via: npm help config

```

### 安装grunt插件

```

[root@study node]# cnpm install -g grunt
npm WARN deprecated coffee-script@1.10.0: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
/opt/install/node/bin/grunt -> /opt/install/node/lib/node_modules/grunt/bin/grunt
+ grunt@1.0.1
added 92 packages in 78.988s
```




