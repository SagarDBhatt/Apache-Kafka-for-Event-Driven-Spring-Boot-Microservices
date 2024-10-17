## Concepts / Theory

- Kafka is distributed messaging system. 
- In RabbitMq or Azure service bus (message queue) architecture, we can have single producer 
and single consumer.
- In Kafka, we can have multiple consumer that can consume the message that producer generates.
- "Kafka Broker" -> is the kafka component which consume, manage and pass the message between 
Kafka producer and consumer. 
- Broker follows Leader-Follower architecture. Each Kafka topic partition assigned a Leader
broker and followers brokers are used for 'in-sync' replica of the messages to provide highest availability of the messages. 
- If leader broker is down then one of the follower broker will become leader.

## Steps to run Kafka

1. Install the Kafka to local machine (c:/kafka).
2. Previously, we use ZooKeeper server to manage Kafka clusters. In latest approach, we use "Kraft" - Kafka Raft to setup kafka cluster. 
3. First we need to generate unique id fork kafka cluster. 

   PS C:\kafka> .\bin\windows\kafka-storage.bat random-uuid
   uLIL59x4SCmNfZ6DJWW2Tg

4. Need to use "uLIL59x4SCmNfZ6DJWW2Tg" uuid to configure the cluster. Below command will configure kafka cluster.

   PS C:\kafka> .\bin\windows\kafka-storage.bat format -t uLIL59x4SCmNfZ6DJWW2Tg -c .\config\kraft\server.properties

5. Start the Kafka-server. Below cmd will start the kafka server on localhost://9092 

   PS C:\kafka> .\bin\windows\kafka-server-start.bat .\config\kraft\server.properties

6. Graceful shutdown of the kafka server. (CTRL + C command will stop the server forcefully. It is recommended to stop the server gracefully.)

   PS C:\kafka> .\bin\windows\kafka-server-stop.bat

7. Create the topic using CLI. The topic "topic_one" is created and registered to bootstrap server / Kafka server "localhost:9092".

   PS C:\kafka>  .\bin\windows\kafka-topics.bat --create --topic topic_one --bootstrap-server localhost:9092

8. See the list of topics registered to kafka server (localhost:9092). "--list" will list all the topics. "--describe" provides more details about the topic.

   PS C:\kafka> .\bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092
   PS C:\kafka> .\bin\windows\kafka-topics.bat --describe --bootstrap-server localhost:9092

9. To delete the topic from kafka server

   PS C:\kafka> .\bin\windows\kafka-topics.bat --delete --topic topic_one --bootstrap-server localhost:9092

10. Create the producer to send the message to the topic.
   (without Key)
   PS C:\kafka> .\bin\windows\kafka-console-producer.bat --bootstrap-server localhost:9092 --topic topic_one

    (with Key - Here mentioned parse.key=true and key-separator as : )
    PS C:\kafka> .\bin\windows\kafka-console-producer.bat --bootstrap-server localhost:9092 --topic topic_one --property "parse.key=true" --property "key.separator=:"

11. Kafka consumer to read the message on the topic. 

   (Reading the message from topic)
   PS C:\kafka> .\bin\windows\kafka-console-consumer.bat --topic topic_one --from-beginning --bootstrap-server localhost:9092

   (Reading the key and value from topic)
   PS C:\kafka> .\bin\windows\kafka-console-consumer.bat --topic topic_one --from-beginning --bootstrap-server localhost:9092 --property print.key=true

NOTE: To consume the messages in order, we need to pass the same key with value from Kafka Producer. This will allow the consumer to consume the messages in order. 

==================================================================================

Create the topic using Kafka Configuration file and push the topic to bootstrap server. 

12. Create a class with @Configuration annotation. Create a method with @Bean annotation which will generate the topic using TopicBuilder to create the topic.

@Configuration
public class KafkaConfig {
    @Bean
    public NewTopic createTopic(){
        return TopicBuilder.name("Product-created-event-topic").build();
    }
}

13. Provide the Kafka bootstrap server details in the application.properties file along with the producer key serializer and value de-serializer. 

spring.kafka.producer.bootstrap-server=[::1]:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.producer.value-serializer=org.springframework.kafka.support.serializer.JsonDeserializer 

14. Validate if the topic is successfully registered with the Kafka bootstrap server by using below command in the CLI. 

Start the Kafka bootstrap server:
    C:\kafka> .\bin\windows\kafka-server-start.bat .\config\kraft\server.properties 

List all the topics attached to this Kafka bootstrap server:
    C:\kafka> .\bin\windows\kafka-topics.bat --list --bootstrap-server localhost:9092
    Picked up JAVA_TOOL_OPTIONS: -Djavax.net.ssl.trustStoreType=Windows-ROOT
    Product-created-event-topic

This should list all the topics that are registered to Kafka bootstrap server. 
