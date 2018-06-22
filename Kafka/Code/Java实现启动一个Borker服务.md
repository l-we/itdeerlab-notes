
### 使用Java编程模拟一个Kafka的服务

[1] 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

创建maven项目的详情 请参考 [ 使用Idea创建一个maven项目](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/UserGuide/%E4%BD%BF%E7%94%A8Idea%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAMaven%E9%A1%B9%E7%9B%AE.md)

> 启动Kafka之前必须使用Zookeeper，请参考[Java程序模拟一个Zookeeper服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Zookeeper/UserGuide/Java%E7%A8%8B%E5%BA%8F%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AAZookeeper%E6%9C%8D%E5%8A%A1.md) 启动一个Zookeeper的服务。确保成功的运行。

[2] 添加依赖

```
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka_2.12</artifactId>
    <version>0.11.0.2</version>
</dependency>
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>0.11.0.2</version>
</dependency>
```

[3] 创建包及启动类

> 在cn/itdeer/创建包kafka，然后在kafka包下创建StartKafkaBroker

```
package cn.itdeer.kafka;

import kafka.server.KafkaConfig;
import kafka.server.KafkaServerStartable;
import org.apache.log4j.Logger;

import java.io.File;
import java.io.IOException;
import java.util.Properties;

/**
 * Description : 启动本地的Kafka进程
 * PackageName : cn.itdeer.kafka
 * ProjectName : Demo
 * CreatorName : itdeer.cn
 * CreateTime : 18-6-22/上午10:41
 */
public class StartKafkaBroker {
    private Logger logger = Logger.getLogger(getClass());

    /**
     * 启动Kafka的主函数
     * @param args
     */
    public static void main(String[] args) throws Exception{
        new StartKafkaBroker().startKafka();
    }

    /**
     * 第二步: 启动Kafka的local的进程
     */

    public void startKafka(){

        final File kafka_dir;

        try {
            kafka_dir = File.createTempFile("zk_kafka", "2");

            if (kafka_dir.delete() && kafka_dir.mkdir()) {

                Properties props = new Properties();
                props.setProperty("host.name", "localhost");
                props.setProperty("port","9092");
                props.setProperty("broker.id", "1");
                props.setProperty("zookeeper.connect", "localhost:2181");
                props.setProperty("log.dirs", kafka_dir.getAbsolutePath());

                props.setProperty("num.partitions", "1");
                props.setProperty("offsets.topic.replication.factor","1");
                props.setProperty("transaction.state.log.replication.factor","1");
                props.setProperty("transaction.state.log.min.isr","1");

                // flush every 1ms
                props.setProperty("log.default.flush.scheduler.interval.ms", "1");
                props.setProperty("log.flush.interval", "1");
                props.setProperty("log.flush.interval.messages", "1");
                props.setProperty("replica.socket.timeout.ms", "1500");
                props.setProperty("auto.create.topics.enable", "true");

                logger.info("Kafka加载配置完成...");

                KafkaConfig kafkaConfig = new KafkaConfig(props);
                KafkaServerStartable kafka = new KafkaServerStartable(kafkaConfig);
                kafka.startup();
                logger.info("Kafka启动成功...... ");
            }
        } catch (IOException e) {
            logger.error("Kafka启动失败......"+e.getMessage());
        }
    }
}

```

[4] 启动验证

```
10:45:34,137  INFO StartKafkaBroker:61 - Kafka加载配置完成...
10:45:34,724  INFO KafkaConfig:223 - KafkaConfig values: 
	advertised.host.name = null
	advertised.listeners = null
	advertised.port = null
	......
	transaction.state.log.num.partitions = 50
	transaction.state.log.replication.factor = 1
	transaction.state.log.segment.bytes = 104857600
	transactional.id.expiration.ms = 604800000
	unclean.leader.election.enable = false
	zookeeper.connect = localhost:2181
	zookeeper.connection.timeout.ms = null
	zookeeper.session.timeout.ms = 6000
	zookeeper.set.acl = false
	zookeeper.sync.time.ms = 2000

10:45:34,801  INFO KafkaServer:70 - starting
.......
10:45:45,745  INFO RequestSendThread:70 - [Controller-1-to-broker-1-send-thread]: Controller 1 connected to localhost:9092 (id: 1 rack: null) for sending state change requests
10:45:45,752  INFO AppInfoParser:83 - Kafka version : 0.11.0.2
10:45:45,752  INFO AppInfoParser:84 - Kafka commitId : 73be1e1168f91ee2
10:45:45,753  INFO KafkaServer:70 - [Kafka Server 1], started
10:45:45,753  INFO StartKafkaBroker:66 - Kafka启动成功...... 
```

> 说明KafkaBroker服务已经启动了，并且监听在9092端口上，可以使用了。接下来使用java 实现一个kafka Producer