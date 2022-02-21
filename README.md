# Kafka

Apache Kafka is an open-source publish-subscribe messaging system which is based on distributed streaming process. It provides real time(~10 msec) transfer of messages from source system to target system. Here source systems are referred to as Producers (write data to Kafka) and target system referred to as Consumers (read data from Kafka). Data in Kafka is only kept for a limited time(default is one week)


## Topics

A topic represent a particular stream of data.
There can be N number of topics


## Partitions

A topic is divided into partitions.
Data stopred in each partition is ordered.
Each message within partition gets an incremental id called Offset. An offset points to last message in partition.
Data of partition 1 at offset 2 might be different from data of partition 2 at offset 2.
Data is immutable (Once data is written in partition, it cannot be changed).
Data is assigned randomly to a partion unless key is provided.

![image](https://user-images.githubusercontent.com/22323263/154833243-a554234b-eb36-4796-80b4-9cac6e4283dc.png)



## Kafka Cluster

A Kafka cluster consists of multiple brokers. 
Here, broker is refer to a Kafka server. Each broker is identified with its id.


## Replication Factor

If the topic have the replication factor of greater than 1, Kafka will create replicas of partition for that topic within Kafka cluster.
Replication Factor cannot be greater than number of brokers in Kafka cluster.
Below example show Topic A with 2 partions and replication factor of 2.

![image](https://user-images.githubusercontent.com/22323263/154833302-c0404ceb-4c16-45b0-aafc-294addad1d05.png)

If one of the broker gets down, then still there exists the replication of that partition in another broker.
Repliation factor should be atleast 3.


## Leaders and Followers

In Kafka Cluster, there are multiple brokers and they have multiple topics and partitions. There is always exist a leader for a particular topic and partition. And other replicas are treated as followers. A Producers always write data to the leader and all other followers will synchronize data with the their leaders. And consumers read data from these replicas.

![image](https://user-images.githubusercontent.com/22323263/154833692-3d20878e-da62-4279-b0f2-b888f66b76bb.png)

If any of the broker consists of partition leader gets down, then one of its replia will be assigned as new leader.
For Example - In the abouve diagram, if Broker 102 get down, Broker 103 Partition 1 Topic A will be assigned as new leader and then new replicas are created in sync of new leader replica.


## Producers Acknowledgement

Producers can choose to receive the following anknowledgement on data writes:
1. acks = 0 : Producers won't wait for acknowledgement. It may lead to data loss.
2. acks = 1 : Producers will wait for the leader anknowledgement. It may lead to limited Data Loss
3. acks = all : Producers will wait for the leader and all the replica's acknowledgement. There is no data loss.


## Producers Message Key

Producer may choose to send key with the message. A key can be a string, number, etc.
If key is null, data is send using Round Robin algorithm i.e. Broker101, then Broker102, then Broker103 and so on.
If key is send then all the messages for that key will always go to the same partition(except for case if number of partition is changed in-between)


## Comsumers Offset

Kafka stores the offsets at which the consumer group has been reading. A consumer group consists of multiple consumers and each consumer in the group read from exclusive partitions.
These offets are stored in Kafka topic named __consumer_offsets.
When a consumer in a group has processed data received from Kafka, it updates the offset value in the Kafka topic. It is done so because if a consumer dies, it will be albe to read back from where it left off.


## Consumers Delivery Semantics

Comsumer can choose when to commit offsets. These are of 3 types:
1. At Most Once : Offset is commited as soon as the message is received. If consumer is unable to process message, then that message cannot be read again and it will loose the message.
2. At Least Once : Offset is committed when the message is processed by the consumer. If consumer is unable to process the message, the message can be read again. It might also led to processing of same message twice.
3. Exactly Once : It is achievable using Kafka Stream API or Apache Spark.
