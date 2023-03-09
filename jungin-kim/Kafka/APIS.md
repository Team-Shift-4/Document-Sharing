# APIS

Kafka는 다섯개의 핵심 API를 제공합니다.

https://godekdls.github.io/Apache%20Kafka/apis/

1. Kafka Cluster에 있는 Topic에 Data Stream을 전송하는 Application을 위한 Producer API
2. Kafka Cluster에 있는 Topic에서 Data Stream을 읽는 Application을 위한 Consumer API
3. Input Topic Data를 읽어 Output Topic에 전송하는 Data Transfer Stream을 위한 Streams API
4. 계속 어떠한 Source System에서 Data를 Pull해서 Kafka로 보내거나 Kafka Data를 다른 Sink System이나 다른 Application으로 Push하는 Connect API
5. Topic, Broker, 다른 Kafka Object를 관리하고 점검할 수 있는 Admin API

Kafka는 모든 기능을 다양한 Programming Language를 지원하는 Client를 가진 언어 독립적인 Protocol을 통해 제공됩니다.
단 Java Client만 Main Kafka Project로 유지하고 있으며 나머지는 독립적인 Open Source Project로 제공합니다.

>   [Java 외의 나머지 Client 목록](https://cwiki.apache.org/confluence/display/KAFKA/Clients)

## Producer API

Application에 Producer API를 사용하면 Kafka Cluster에 있는 Topic으로 Data Stream을 전송할 수 있습니다.

>   [Producer를 활용하는 예제](https://kafka.apache.org/27/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html)

Maven에서 아래와 같이 의존성을 추가하면 됩니다.

```xml
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifiactId>kafka-clients</artifiactId>
    <version>2.7.0</version>
</dependency>
```

## Consumer API

Application에 Consumer API를 사용하면 Kafka Cluster에 있는 Topic에서 Data Stream을 읽을 수 있습니다.

>   [Consumer를 활용하는 예제](https://kafka.apache.org/27/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html)

## Streams API

## Connect API

## Admin API