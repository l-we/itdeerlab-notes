
### Java实现启动一个Consumer

[1] 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

创建maven项目的详情 请参考 [ 使用Idea创建一个maven项目](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/UserGuide/%E4%BD%BF%E7%94%A8Idea%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAMaven%E9%A1%B9%E7%9B%AE.md)

> 启动Kafka之前必须使用Zookeeper，请参考[Java程序模拟一个Zookeeper服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Zookeeper/UserGuide/Java%E7%A8%8B%E5%BA%8F%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AAZookeeper%E6%9C%8D%E5%8A%A1.md) 启动一个Zookeeper的服务。确保成功的运行。

> 启动KafkaBroker服务，请参考[Java实现启动一个Borker服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Kafka/Code/Java%E5%AE%9E%E7%8E%B0%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AABorker%E6%9C%8D%E5%8A%A1.md)

> 启动KafkaProducer服务，请参考[Java实现启动一个Producer](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Kafka/Code/Java%E5%AE%9E%E7%8E%B0%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AAProducer.md)


[2] 创建包及启动类

> 在cn/itdeer/kafka，然后在kafka包下创建StartKafkaConsumer

```
package cn.itdeer.kafka;

import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.log4j.Logger;

import java.util.Collections;
import java.util.Properties;

/**
 * Description : Kafka生产者
 * PackageName : cn.itdeer.kafka
 * ProjectName : Demo
 * CreatorName : itdeer.cn
 * CreateTime : 18-6-25/下午6:00
 */
public class StartKafkaConsumer {

    Logger logger = Logger.getLogger(StartKafkaConsumer.class);
    private String topicName = "itdeer_demo1";

    public void startConsumer(){

        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("group.id", "demo");
        props.put("enable.auto.commit", "true");
        props.put("auto.commit.interval.ms", "1000");
        props.put("key.deserializer", Serdes.String().deserializer().getClass().getName());
        props.put("value.deserializer", Serdes.String().deserializer().getClass().getName());

        KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(props);

        consumer.subscribe(Collections.singletonList(topicName));

        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(10);
            for (ConsumerRecord<String, String> record : records)
                logger.info("offset = " + record.offset() + ", key = " + record.key() + ", value = " + record.value());
        }
    }

    /**
     * 测试主函数
     * @param args
     */
    public static void main(String[] args) {
        new StartKafkaConsumer().startConsumer();
    }
}

```

[3] 启动验证

```
    ......
	auto.commit.interval.ms = 1000
	auto.offset.reset = latest
	bootstrap.servers = [localhost:9092]
	check.crcs = true
	client.id = 
	connections.max.idle.ms = 540000
	enable.auto.commit = true
	exclude.internal.topics = true
	fetch.max.bytes = 52428800
	fetch.max.wait.ms = 500
	fetch.min.bytes = 1
	group.id = demo
	heartbeat.interval.ms = 3000
	interceptor.classes = null
	internal.leave.group.on.close = true
	isolation.level = read_uncommitted
	key.deserializer = class org.apache.kafka.common.serialization.StringDeserializer
    ......
	sasl.kerberos.min.time.before.relogin = 60000
	sasl.kerberos.service.name = null
	sasl.kerberos.ticket.renew.jitter = 0.05
	sasl.kerberos.ticket.renew.window.factor = 0.8
	sasl.mechanism = GSSAPI
	security.protocol = PLAINTEXT
	send.buffer.bytes = 131072
	session.timeout.ms = 10000
	ssl.cipher.suites = null
	ssl.enabled.protocols = [TLSv1.2, TLSv1.1, TLSv1]
	ssl.endpoint.identification.algorithm = null
	ssl.key.password = null
	ssl.keymanager.algorithm = SunX509
	ssl.keystore.location = null
	ssl.keystore.password = null
	ssl.keystore.type = JKS
	ssl.protocol = TLS
	ssl.provider = null
	ssl.secure.random.implementation = null
	ssl.trustmanager.algorithm = PKIX
	ssl.truststore.location = null
	ssl.truststore.password = null
	ssl.truststore.type = JKS
	value.deserializer = class org.apache.kafka.common.serialization.StringDeserializer

18:09:22,960  INFO AppInfoParser:83 - Kafka version : 0.11.0.2
18:09:22,962  INFO AppInfoParser:84 - Kafka commitId : 73be1e1168f91ee2
18:09:23,096  INFO AbstractCoordinator:607 - Discovered coordinator localhost:9092 (id: 2147483646 rack: null) for group demo.
18:09:23,098  INFO ConsumerCoordinator:419 - Revoking previously assigned partitions [] for group demo
18:09:23,098  INFO AbstractCoordinator:442 - (Re-)joining group demo
18:09:31,896  INFO AbstractCoordinator:409 - Successfully joined group demo with generation 2
18:09:31,897  INFO ConsumerCoordinator:262 - Setting newly assigned partitions [itdeer_demo1-0] for group demo
18:09:43,941  INFO StartKafkaConsumer:41 - offset = 0, key = itdeer, value = 0
18:09:46,890  INFO StartKafkaConsumer:41 - offset = 1, key = itdeer, value = 1

```

> 说明生产者服务已经开始工作了，开始消费topic itdeer_demo1的数据。