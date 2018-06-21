
### 使用Java编程模拟一个Zookeeper的服务

> 我们做大数据研发很多时候使用到Zookeeper，我们在公司有相关的环境，可以使用真实的Zookeeper服务。但是有时候在家就可能没有这样的环境，而且自己的机器配置又不高，不能搭建环境，这时候自己模拟一个是最方便了，使用的是时候启动起来即可，不用的时候关闭就行。下面我会把相关的代码一并贴到文章中，供大家参阅和自己使用的时候方便用。


### 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

[1] 开始创建项目

![2018621175855](http://panrhkqz9.bkt.clouddn.com/2018621175855.png)

创建maven项目的详情 请参考 [ 使用Idea创建一个maven项目](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/UserGuide/%E4%BD%BF%E7%94%A8Idea%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAMaven%E9%A1%B9%E7%9B%AE.md)

[2] 添加依赖

```
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.6</version>
</dependency>
```

[3] 创建包及启动类

![2018621235229](http://panrhkqz9.bkt.clouddn.com/2018621235229.png)

```
package cn.itdeer;

import org.apache.zookeeper.server.ServerConfig;
import org.apache.zookeeper.server.ZooKeeperServerMain;
import org.apache.zookeeper.server.quorum.QuorumPeerConfig;

import java.io.File;
import java.io.IOException;
import java.util.Properties;

/**
 * Description : 启动一个Zookeeper服务
 * PackageName : cn.itdeer
 * ProjectName : Demo
 * CreatorName : itdeer.cn
 * CreateTime : 2018/6/21/23:51
 */
public class StartZookeeper {

    private ServerConfig config = new ServerConfig();

    /**
     * 启动Zookeeper的主函数
     * @param args
     * @throws Exception
     */
    public static void main(String[] args) throws Exception{
        new StartZookeeper().startZookeeper();
    }

    /**
     * 启动程序
     * @throws Exception
     */
    public void startZookeeper() throws Exception{
        File zookeeper_dir = File.createTempFile("zookeeper", "test");
        if (zookeeper_dir.delete() && zookeeper_dir.mkdir()) {

            /*
             * 加载配置信息
             */
            Properties zkProperties = new Properties();
            zkProperties.setProperty("dataDir", zookeeper_dir.getAbsolutePath());
            zkProperties.setProperty("clientPort", "2181");
            zkProperties.setProperty("tickTime", "4000");
            zkProperties.setProperty("initLimit", "10");
            zkProperties.setProperty("syncLimit", "5");

            QuorumPeerConfig quorumConfiguration = new QuorumPeerConfig();
            quorumConfiguration.parseProperties(zkProperties);


            config.readFrom(quorumConfiguration);

            System.out.println("配置加载完成.....");
            /*
             * 启动新线程
             */
            new Thread("zookeeper") {
                public void run() {
                    try {
                        System.out.println("启动 Local ZooKeeper .....");
                        new ZooKeeperServerMain().runFromConfig(config);
                    } catch (IOException e) {
                        System.out.println("启动 Local ZooKeeper Failed:" + e.getStackTrace());
                        e.printStackTrace(System.err);
                    }
                }
            }.start();
        } else {
            System.out.println("Failed to delete or create domain dir for Zookeeper");
        }
    }
}

```

[4] 启动验证

![2018622000](http://panrhkqz9.bkt.clouddn.com/2018622000.png)

[5] 检测端口

![20186220329](http://panrhkqz9.bkt.clouddn.com/20186220329.png)