
### 使用Java编程模拟一个Zookeeper的服务

> 我们做大数据研发很多时候使用到Zookeeper，我们在公司有相关的环境，可以使用真实的Zookeeper服务。但是有时候在家就可能没有这样的环境，而且自己的机器配置又不高，不能搭建环境，这时候自己模拟一个是最方便了，使用的是时候启动起来即可，不用的时候关闭就行。下面我会把相关的代码一并贴到文章中，供大家参阅和自己使用的时候方便用。


### 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

[1] 开始创建项目

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
package cn.itdeer.zookeeper;

import org.apache.log4j.Logger;
import org.apache.zookeeper.server.ServerConfig;
import org.apache.zookeeper.server.ZooKeeperServerMain;
import org.apache.zookeeper.server.quorum.QuorumPeerConfig;

import java.io.File;
import java.io.IOException;
import java.util.Properties;

/**
 * Description : 启动一个Zookeeper服务
 * PackageName : cn.itdeer.zookeeper
 * ProjectName : Demo
 * CreatorName : itdeer.cn
 * CreateTime : 18-6-22/上午10:22
 */
public class StartZookeeper {

    private Logger logger = Logger.getLogger(getClass());
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

            logger.info("配置加载完成.....");

            /*
             * 启动新线程
             */
            new Thread("zookeeper") {
                public void run() {
                    try {
                        logger.info("启动 Local ZooKeeper .....");
                        new ZooKeeperServerMain().runFromConfig(config);
                    } catch (IOException e) {
                        logger.error("启动 Local ZooKeeper Failed:" + e.getStackTrace());
                        e.printStackTrace(System.err);
                    }
                }
            }.start();
        } else {
            logger.warn("Failed to delete or create domain dir for Zookeeper");
        }
    }
}

```

[4] 启动验证

```
......
10:27:27,874  INFO StartZookeeper:59 - 配置加载完成.....
10:27:27,876  INFO StartZookeeper:67 - 启动 Local ZooKeeper .....
10:27:27,878  INFO ZooKeeperServerMain:95 - Starting server
10:27:37,914  INFO ZooKeeperServer:100 - Server environment:zookeeper.version=3.4.6--1, built on 04/18/2018 04:22 GMT
10:27:37,914  INFO ZooKeeperServer:100 - Server environment:host.name=itdeer
10:27:37,916  INFO ZooKeeperServer:100 - Server environment:java.version=1.8.0_73
10:27:37,917  INFO ZooKeeperServer:100 - Server environment:java.vendor=Oracle Corporation

......

10:27:37,917  INFO ZooKeeperServer:100 - Server 
10:27:37,927  INFO ZooKeeperServer:755 - tickTime set to 4000
10:27:37,927  INFO ZooKeeperServer:764 - minSessionTimeout set to -1
10:27:37,927  INFO ZooKeeperServer:773 - maxSessionTimeout set to -1
10:27:37,941  INFO NIOServerCnxnFactory:94 - binding to port 0.0.0.0/0.0.0.0:2181
```

[5] 检测端口

![20186220329](http://panrhkqz9.bkt.clouddn.com/20186220329.png)

> 说明Zookeeper服务已经启动了，并且监听在2181端口上，可以使用了。