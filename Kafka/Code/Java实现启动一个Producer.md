
### 使用Java编程模拟一个Kafka的服务

[1] 开发环境

> Idea Maven 这里就不赘述开发环境的搭建了。

创建maven项目的详情 请参考 [ 使用Idea创建一个maven项目](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Tools/UserGuide/%E4%BD%BF%E7%94%A8Idea%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAMaven%E9%A1%B9%E7%9B%AE.md)

> 启动Kafka之前必须使用Zookeeper，请参考[Java程序模拟一个Zookeeper服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Zookeeper/UserGuide/Java%E7%A8%8B%E5%BA%8F%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AAZookeeper%E6%9C%8D%E5%8A%A1.md) 启动一个Zookeeper的服务。确保成功的运行。

> 启动KafkaBroker服务，请参考[Java实现启动一个Borker服务](https://github.com/ItdeerLab/itdeerlab-notes/blob/notes/Kafka/Code/Java%E5%AE%9E%E7%8E%B0%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AABorker%E6%9C%8D%E5%8A%A1.md)


[3] 创建包及启动类

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

[4] 启动验证

```
/opt/install/jdk-1.8/bin/java -javaagent:/home/itdeer/idea/lib/idea_rt.jar=32853:/home/itdeer/idea/bin -Dfile.encoding=UTF-8 -classpath /opt/install/jdk-1.8/jre/lib/charsets.jar:/opt/install/jdk-1.8/jre/lib/deploy.jar:/opt/install/jdk-1.8/jre/lib/ext/cldrdata.jar:/opt/install/jdk-1.8/jre/lib/ext/dnsns.jar:/opt/install/jdk-1.8/jre/lib/ext/jaccess.jar:/opt/install/jdk-1.8/jre/lib/ext/jfxrt.jar:/opt/install/jdk-1.8/jre/lib/ext/localedata.jar:/opt/install/jdk-1.8/jre/lib/ext/nashorn.jar:/opt/install/jdk-1.8/jre/lib/ext/sunec.jar:/opt/install/jdk-1.8/jre/lib/ext/sunjce_provider.jar:/opt/install/jdk-1.8/jre/lib/ext/sunpkcs11.jar:/opt/install/jdk-1.8/jre/lib/ext/zipfs.jar:/opt/install/jdk-1.8/jre/lib/javaws.jar:/opt/install/jdk-1.8/jre/lib/jce.jar:/opt/install/jdk-1.8/jre/lib/jfr.jar:/opt/install/jdk-1.8/jre/lib/jfxswt.jar:/opt/install/jdk-1.8/jre/lib/jsse.jar:/opt/install/jdk-1.8/jre/lib/management-agent.jar:/opt/install/jdk-1.8/jre/lib/plugin.jar:/opt/install/jdk-1.8/jre/lib/resources.jar:/opt/install/jdk-1.8/jre/lib/rt.jar:/work/workspace/idea/Demo/target/classes:/opt/install/maven-3.3.9/working/junit/junit/4.12/junit-4.12.jar:/opt/install/maven-3.3.9/working/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar:/opt/install/maven-3.3.9/working/org/slf4j/slf4j-api/1.7.22/slf4j-api-1.7.22.jar:/opt/install/maven-3.3.9/working/org/slf4j/slf4j-log4j12/1.7.22/slf4j-log4j12-1.7.22.jar:/opt/install/maven-3.3.9/working/log4j/log4j/1.2.17/log4j-1.2.17.jar:/opt/install/maven-3.3.9/working/org/apache/zookeeper/zookeeper/3.4.6/zookeeper-3.4.6.jar:/opt/install/maven-3.3.9/working/jline/jline/0.9.94/jline-0.9.94.jar:/opt/install/maven-3.3.9/working/io/netty/netty/3.7.0.Final/netty-3.7.0.Final.jar:/opt/install/maven-3.3.9/working/org/apache/kafka/kafka_2.12/0.11.0.2/kafka_2.12-0.11.0.2.jar:/opt/install/maven-3.3.9/working/net/sf/jopt-simple/jopt-simple/5.0.3/jopt-simple-5.0.3.jar:/opt/install/maven-3.3.9/working/com/yammer/metrics/metrics-core/2.2.0/metrics-core-2.2.0.jar:/opt/install/maven-3.3.9/working/org/scala-lang/scala-library/2.12.2/scala-library-2.12.2.jar:/opt/install/maven-3.3.9/working/com/101tec/zkclient/0.10/zkclient-0.10.jar:/opt/install/maven-3.3.9/working/org/scala-lang/modules/scala-parser-combinators_2.12/1.0.4/scala-parser-combinators_2.12-1.0.4.jar:/opt/install/maven-3.3.9/working/org/apache/kafka/kafka-clients/0.11.0.2/kafka-clients-0.11.0.2.jar:/opt/install/maven-3.3.9/working/net/jpountz/lz4/lz4/1.3.0/lz4-1.3.0.jar:/opt/install/maven-3.3.9/working/org/xerial/snappy/snappy-java/1.1.2.6/snappy-java-1.1.2.6.jar cn.itdeer.kafka.StartKafkaProducer
18:33:03,803  INFO ProducerConfig:223 - ProducerConfig values: 
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
	max.request.size = 1048576
	metadata.max.age.ms = 300000
	metric.reporters = []
	metrics.num.samples = 2
	metrics.recording.level = INFO
	metrics.sample.window.ms = 30000
	partitioner.class = class org.apache.kafka.clients.producer.internals.DefaultPartitioner
	receive.buffer.bytes = 32768
	reconnect.backoff.max.ms = 1000
	reconnect.backoff.ms = 50
	request.timeout.ms = 30000
	retries = 0
	retry.backoff.ms = 100
	sasl.jaas.config = null
	sasl.kerberos.kinit.cmd = /usr/bin/kinit
	sasl.kerberos.min.time.before.relogin = 60000
	sasl.kerberos.service.name = null
	sasl.kerberos.ticket.renew.jitter = 0.05
	sasl.kerberos.ticket.renew.window.factor = 0.8
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

> 说明KafkaBroker服务已经启动了，并且监听在9092端口上，可以使用了。接下来使用java 实现一个kafka Producer