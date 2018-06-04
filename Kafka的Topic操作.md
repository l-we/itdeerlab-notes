Kafka官方提供了两个脚本来管理topic，包括topic的增删改查。其中kafka-topics.sh负责topic的创建与删除；kafka-configs.sh脚本负责topic的修改和查询，但很多用户都更加倾向于使用程序API的方式对topic进行操作。
 
上一篇文章中提到了如何使用客户端协议(client protocol)来创建topic，本文则使用服务器端的Java API对topic进行增删改查。开始之前，需要明确的是，下面的代码需要引入kafka-core的依赖，以kafka 0.10.2版本为例：
Maven版本
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka_2.10</artifactId>
    <version>0.10.2.0</version>
</dependency>
 
Gradle版本
compile group: 'org.apache.kafka', name: 'kafka_2.10', version: '0.10.2.0'
 
创建topic
ZkUtils zkUtils = ZkUtils.apply("localhost:2181", 30000, 30000, JaasUtils.isZkSecurityEnabled());
// 创建一个单分区单副本名为t1的topic
AdminUtils.createTopic(zkUtils, "t1", 1, 1, new Properties(), RackAwareMode.Enforced$.MODULE$);
zkUtils.close();
删除topic

ZkUtils zkUtils = ZkUtils.apply("localhost:2181", 30000, 30000, JaasUtils.isZkSecurityEnabled());
// 删除topic 't1'
AdminUtils.deleteTopic(zkUtils, "t1");
zkUtils.close();
比较遗憾地是，不管是创建topic还是删除topic，目前Kafka实现的方式都是后台异步操作的，而且没有提供任何回调机制或返回任何结果给用户，所以用户除了捕获异常以及查询topic状态之外似乎并没有特别好的办法可以检测操作是否成功。

查询topic

按 Ctrl+C 复制代码

ZkUtils zkUtils = ZkUtils.apply("localhost:2181", 30000, 30000, JaasUtils.isZkSecurityEnabled());
// 获取topic 'test'的topic属性属性
Properties props = AdminUtils.fetchEntityConfig(zkUtils, ConfigType.Topic(), "test");
// 查询topic-level属性
Iterator it = props.entrySet().iterator();
while(it.hasNext()){
    Map.Entry entry=(Map.Entry)it.next();
    Object key = entry.getKey();
    Object value = entry.getValue();
    System.out.println(key + " = " + value);
}
zkUtils.close();
按 Ctrl+C 复制代码
修改topic

按 Ctrl+C 复制代码

ZkUtils zkUtils = ZkUtils.apply("localhost:2181", 30000, 30000, JaasUtils.isZkSecurityEnabled());
Properties props = AdminUtils.fetchEntityConfig(zkUtils, ConfigType.Topic(), "test");
// 增加topic级别属性
props.put("min.cleanable.dirty.ratio", "0.3");
// 删除topic级别属性
props.remove("max.message.bytes");
// 修改topic 'test'的属性
AdminUtils.changeTopicConfig(zkUtils, "test", props);
zkUtils.close();
按 Ctrl+C 复制代码