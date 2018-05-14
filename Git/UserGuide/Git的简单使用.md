> 这里使用一个项目做演示,这个项目只有一个README的文件

### Clone一个项目代码

> 把项目Clone到根目录下

```
[root@study /]# git clone http://Demo@192.168.0.236/demo/DemoProject.git

Cloning into 'DemoProject'...
Password for 'http://Demo@192.168.0.236': 
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
```

> 进入项目目录

```
[root@study /]# cd DemoProject/

[root@study DemoProject]# ls
README.md
```


### 创建一个新的分支

> 查看默认分支 默认分支为master

```
[root@study DemoProject]# git branch
* master
```

> 创建新分支 分支名称为demo 并从master分支checkout出来

```
[root@study DemoProject]# git branch demo

[root@study DemoProject]# git checkout demo

Switched to branch 'demo'
```

> 提交新内容test.txt到demo分支上

```
[root@study DemoProject]# touch test.txt

[root@study DemoProject]# git add test.txt

[root@study DemoProject]# git commit -m "add test"

[demo 189c9b0] add test
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt

[root@study DemoProject]# git push -u origin demo
Password for 'http://Demo@192.168.0.236': 我的git的密码
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 264 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: To create a merge request for demo, visit:
remote:   http://192.168.0.236/demo/DemoProject/merge_requests/new?merge_request%5Bsource_branch%5D=demo
remote: 
To http://Demo@192.168.0.236/demo/DemoProject.git
 * [new branch]      demo -> demo
Branch demo set up to track remote branch demo from origin.
```

> 把这个test.txt 提交到了demo这个分支上，master上不会有

> 查看分支

```
[root@study DemoProject]# git branch
* demo
  master
```


### Git分支删除

> 首先确保当前不在要删除的分支上

```
[root@study DemoProject]# git checkout master
Switched to branch 'master'

[root@study DemoProject]# git branch
  demo
* master
```

> git 删除本地分支

```
[root@study DemoProject]# git branch -D demo
Deleted branch demo (was 189c9b0).

[root@study DemoProject]# git branch
* master

```

> git 删除远程分支

```
[root@study DemoProject]# git push origin :demo

Password for 'http://Demo@192.168.0.236': 我的git的密码
To http://Demo@192.168.0.236/demo/DemoProject.git
 - [deleted]         demo

```


### git修改默认编辑器

> 命令配置

```
[root@study DemoProject]# git config --global core.editor "vim"
```

> 修改配置文件

```
[root@study DemoProject]# vim ~/.gitconfig

[core]
        editor = vim
```