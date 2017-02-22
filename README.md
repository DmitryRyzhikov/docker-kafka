[Original link](https://github.com/wurstmeister/kafka-docker)
[Kafka tutorial](https://www.tutorialspoint.com/apache_kafka/apache_kafka_basic_operations.htm)
[Zookeper tutorial](https://www.tutorialspoint.com/zookeeper/zookeeper_installation.htm)

File docker-compose will run KAFKA with default settings. KAFKA should be available on 192.168.99.100:9092. Zookeper should be available on 192.168.99.100:2181

## To run 
$ docker-compose up -d

## Access and execute commands
To get access to running containers (with correct container ID:  docker ps)
docker exec -it 5207587d116b /bin/bash
* Check kafka location (opt/kafka/bin) and go there:
ps aux | grep kafka

cd /opt/kafka/bin
* List of topics 


    ./kafka-topics.sh --list --zookeeper zookeeper:2181
* Create topic (1 partition, 1 replica) 


    ./kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1  --partitions 1 --topic Hello-Kafka
* Add messages to topic. For this start producer and type some messages after this.


    ./kafka-console-producer.sh --broker-list localhost:9092 --topic Hello-Kafka
* Consume messages. For this start consumer and give topic and offset

    
    ./kafka-console-consumer.sh --zookeeper zookeeper:2181 --topic Hello-Kafka --from-beginning
* Delete topic


    ./kafka-topics.sh --zookeeper zookeeper:2181 --delete --topic Hello-kafka
* Modify topic
    
    ./kafka-topics.sh --zookeeper zookeeper:2181 --alter --topic Hello-kafka --partitions 2
    
* Run multiple brokers on single node

    
    
## Necessary configurations
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 192.168.99.100
      KAFKA_ADVERTISED_PORT: 9092
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      
KAFKA_ADVERTISED_HOST_NAME - the same as IP of docker virtual machine (check: docker-machine ip)
KAFKA_ADVERTISED_PORT - without this on start kafka throws an exception
KAFKA_ZOOKEEPER_CONNECT - host and port of zookeeper (should run in outher container with binded port)

## Optional configurations
    environment:
      KAFKA_CREATE_TOPICS: "Topic1:1:3,Topic2:1:1:compact"
     
KAFKA_CREATE_TOPICS will create default topics. In example above: Topic 1 will have 1 partition and 3 replicas, Topic 2 will have 1 partition, 1 replica and a cleanup.policy set to compact
