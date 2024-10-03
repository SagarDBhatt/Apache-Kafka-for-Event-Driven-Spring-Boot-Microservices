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
