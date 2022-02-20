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
