### 使用VisualStudioCode编写MarkDown的文档，高效存储图片

> 程序员，工程师都是少不了写文档，尤其是MarkDown的文档，有一个顺手的编写工具那是程序员追求（说白了就是懒....）

> 简单聊一下吧，原来啊，我是自己写博客系统，然后把图片上传到自己的服务器上，但是因为工资低，买的云主机基本上是最低配的，很多时候浏览个文章图片都得等一会能加载出来。后来，我把所有的图片放到了Git上，只是方便了修改操作等等，速度上好像比原来好那么点，凑合着用吧。

> 最近发现有一些更加好用的工具，只是之前从来没有去想过去用，现在想想干嘛不用，自己去搞还搞得不好，干嘛不用比较成熟的东西呢。所以，也就有了今天这个文章。本章节介绍的是，在写MarkDown时把图片快捷的放到云存储上，自己的博客能快速的引用到。

> 下面容我慢慢道来，按照我的步骤走，简单省力。

### 安装VisualStudioCode软件

> 在这里不详细介绍VisualStudioCode软件的安装，如有需要可以查看相关的文档

[1] 在Window下安装方式 详见 [Window下安装VisualStudioCode](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/SoftUtils/Window%E4%B8%8B%E5%AE%89%E8%A3%85VisualStudioCode.md)

[2] 在Centos7.2下安装方式 详见 [Centos7.2下安装VisualStudioCode](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/SoftUtils/Centos7.2%E5%AE%89%E8%A3%85VisualStudioCode.md)

### 安装插件

[1] 打开插件商店

![2018621224537](http://note.itdeer.cn/2018621224537.png)

[2] 安装七牛云平台与VisualStudioCode的插件（插件名称：paste image to qiniu）

![2018621224410](http://note.itdeer.cn/2018621224410.png)

[3] 安装方便的快捷键用于编辑Markdown（.md，.markdown）文件的插件（插件名称：Markdown Shortcuts）

![2018621224758](http://note.itdeer.cn/2018621224758.png)

> 安装完成之后需要重新加载，或者关闭之后再打开即可

### 申请七牛云账号

[1] 申请账号

 - 百度进行搜索

![201862122588](http://note.itdeer.cn/201862122588.png)

![2018621225853](http://note.itdeer.cn/2018621225853.png)

![2018621225948](http://note.itdeer.cn/2018621225948.png)

![201862123011](http://note.itdeer.cn/201862123011.png)

 - 根据自己的邮箱进行验证

![201862123128](http://note.itdeer.cn/201862123128.png)

[2] 实名认证

![201862123157](http://note.itdeer.cn/201862123157.png)

 - 系统会提示你认证，这里我已经进行过认证了，所以截图是没有办法截了，下面直接口述吧。

> 实名认证需要一张手持身份证的正面照片和一张手持身份证反面的照片，然后填写一些必要的信息（包括身份证号等）,提交等待审批。审批完成之后会短信和邮件通知。

[3] 支付宝认证

> 审批完成之后，需要使用支付宝进行扫码，然后进行同意，之后就可以使用了。

[4] 创建存储空间

![201862123719](http://note.itdeer.cn/201862123719.png)

![201862123753](http://note.itdeer.cn/201862123753.png)

![2018621231050](http://note.itdeer.cn/2018621231050.png)

![2018621231137](http://note.itdeer.cn/2018621231137.png)

> 为外链的地址，当然也可以使用自己的域名，这个就自己去捯饬吧，我就不详细说了，因为七牛云的文档是相当的完善了。

### 修改配置

[1] 打开配置文件

![201862122521](http://note.itdeer.cn/201862122521.png)

[2] 查询有关qiniu 的配置项

![201862122539](http://note.itdeer.cn/201862122539.png)

[3] 复制配置

![2018621225348](http://note.itdeer.cn/2018621225348.png)

[4] 更改配置信息

```
{
    "files.autoSave": "afterDelay",
    "workbench.colorTheme": "Visual Studio Dark",
    "workbench.startupEditor": "newUntitledFile",
    "explorer.confirmDelete": false,
    "git.autofetch": true,
    "git.confirmSync": false,
    "git.enableSmartCommit": true,


    // 一个有效的七牛 AccessKey 签名授权。
    "pasteImageToQiniu.access_key": "4Srem4jN85***********bih-mmen1IF4A_vMAmd",

    // 七牛图片上传空间。
    "pasteImageToQiniu.bucket": "itdeerlab-notes",

    // 七牛图床域名。
    "pasteImageToQiniu.domain": "http://panr.******bkt.clouddn.com",

    // 图片本地保存位置
    "pasteImageToQiniu.localPath": "../../itdeerlab-notes-pic",

    // 七牛图片上传路径，参数化命名。
    "pasteImageToQiniu.remotePath": "${dateTime}",

    // 一个有效的七牛 SecretKey 签名授权。
    "pasteImageToQiniu.secret_key": "JcCYw7Z-************2MSX_HL0d"
}
```

> 以上是我个人的存储空间的配置，你需要更改成自己的配置

### 愉快的写文章

> 怎么愉快的写文章呢，就是，当使用QQ的截图快捷键之后，截出来一个图片，点击完成，在需要VSCode的MarkDown文章的插入的位置 使用Ctrl + Alt + V 你刚刚截的图片就会上传到上面创建的存储空间中，文章中返回了，这张图片的访问地址。

> 七牛这一点做的是相当的好，使用比较方便，文档比较全面，关键是还免费，10G的永久免费空间，只存储图片的话，基本是用不完的，同时提供了每月上万次的API访问请求，基本上够用的。

> 下面的地址是VSCode的关于MarkDown的插件，有需要的可以自己去查询安装（https://marketplace.visualstudio.com/search?term=markdown&target=VSCode&category=Other&sortBy=Relevance）
