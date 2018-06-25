
### Java实现启动一个Producer

[1] 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

创建maven项目的详情 请参考 [ 使用Idea创建一个maven项目](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/UserGuide/%E4%BD%BF%E7%94%A8Idea%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAMaven%E9%A1%B9%E7%9B%AE.md)

> 启动Kafka之前必须使用Zookeeper，请参考[Java程序模拟一个Zookeeper服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Zookeeper/UserGuide/Java%E7%A8%8B%E5%BA%8F%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AAZookeeper%E6%9C%8D%E5%8A%A1.md) 启动一个Zookeeper的服务。确保成功的运行。

> 启动KafkaBroker服务，请参考[Java实现启动一个Borker服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Kafka/Code/Java%E5%AE%9E%E7%8E%B0%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AABorker%E6%9C%8D%E5%8A%A1.md)


[2] 创建包及启动类

> 在cn/itdeer/kafka，然后在kafka包下创建StartKafkaProducer

```
package cn.itdeer.kafka;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.log4j.Logger;

import java.util.Properties;

/**
 * Description : Kafka生产者
 * PackageName : cn.itdeer.kafka
 * ProjectName : Demo
 * CreatorName : itdeer.cn
 * CreateTime : 18-6-22/下午12:59
 */
public class StartKafkaProducer {

    Logger logger = Logger.getLogger(StartKafkaProducer.class);
    private String topicName = "itdeer_demo1";

    public void startProducer(){

        Properties producerConfig = new Properties();

        producerConfig.put("bootstrap.servers", "localhost:9092");
        producerConfig.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, Serdes.String().serializer().getClass().getName());
        producerConfig.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, Serdes.String().serializer().getClass().getName());

        KafkaProducer producer = new KafkaProducer<String, String>(producerConfig);

        int number = 0;

        while(true) {
            producer.send(new ProducerRecord<String, String>(topicName, "itdeer", number + ""));
            logger.info("Topic: " + topicName + "   Key: itdeer Value:" + number);
            number ++;
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 测试主函数
     * @param args
     */
    public static void main(String[] args) {
        new StartKafkaProducer().startProducer();
    }

}


```

[3] 启动验证

```
	......
	acks = 1
	batch.size = 16384
	bootstrap.servers = [localhost:9092]
	buffer.memory = 33554432
	client.id = 
	compression.type = none
	connections.max.idle.ms = 540000
	enable.idempotence = false
	interceptor.classes = null
	key.serializer = class org.apache.kafka.common.serialization.StringSerializer
	linger.ms = 0
	max.block.ms = 60000
	max.in.flight.requests.per.connection = 5
	......
	sasl.mechanism = GSSAPI
	security.protocol = PLAINTEXT
	send.buffer.bytes = 131072
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
	transaction.timeout.ms = 60000
	transactional.id = null
	value.serializer = class org.apache.kafka.common.serialization.StringSerializer

18:33:03,904  INFO AppInfoParser:83 - Kafka version : 0.11.0.2
18:33:03,904  INFO AppInfoParser:84 - Kafka commitId : 73be1e1168f91ee2
18:33:04,137  WARN NetworkClient:846 - Error while fetching metadata with correlation id 1 : {itdeer_demo1=LEADER_NOT_AVAILABLE}
18:33:04,248  WARN NetworkClient:846 - Error while fetching metadata with correlation id 3 : {itdeer_demo1=LEADER_NOT_AVAILABLE}
18:33:04,367  INFO StartKafkaProducer:37 - Topic: itdeer_demo1   Key: itdeer Value:0
18:33:07,367  INFO StartKafkaProducer:37 - Topic: itdeer_demo1   Key: itdeer Value:1
```

> 说明Kafka生产者服务已经启动了，已经开始生产数据了，生产的数据放置topic itdeer_demo1。接下来使用java 实现一个kafka Consumer