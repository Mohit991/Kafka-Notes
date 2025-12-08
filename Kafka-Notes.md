# Kafka Notes
## Introduction
- Open Source Distributed Event Streaming Platform
- Handles data that is constantly being generated and needs to ve processed as it comes, without delays.
- Eg. social media plaforms generate events in the forms of likes, comments etc, kafka will collect these events, store these events and distributes these events to multiple services usually microservies.
- Kafka ensures the smooth flow of data from source to destination.
- Async messaging
- Producer will produce data and publish to the kafka topic, consumer will consume data from kafka topic. This makes it async. REST API calls are sync but kafka is async.
- Fault tolerance and scalability. Decoupling of producers and consumers.

## Terminologies
### Kafka Broker
- A server on which kafka is running.

### Kafka Cluster
- Group of kafka brokers.

### Kafka Producer
- Writes new data into kafka cluster

### Kafka Consumer
- Consumes/reads data from kafka cluster.

### Zookepeer
- Manages kafka cluster health.

### Kafka Connect
- Connects external sources of data with kafka. You can bring data from a database into kafka cluster using kafka connect. Any external source data can be brought to kafka cluster without writing any code using kafka connect.

### Kafka Stream
- Used for data transformation.
- Take some data from kafka cluster, make some changes to this data and put it in the cluster Kafka Topic

### Kafka Topic
- A Kafka topic is a category or feed name to which records (messages) are published in Apache Kafka

**Key Concepts**

**What is it?**
	•	A logical channel for organizing and storing streams of records
	•	Similar to a table in a database or a folder for messages
	•	Messages are published to topics and consumed from topics


**Characteristics**

**Durability**:
	•	Messages are persisted to disk
	•	Retained for a configurable period (e.g., 7 days, 30 days, or indefinitely)



**Partitioning**:
	•	Topics are divided into partitions for scalability
	•	Each partition is an ordered, immutable sequence of records
	•	Partitions enable parallel processing



**Ordering**:
	•	Messages within a partition are ordered
	•	Each message gets a sequential ID called an offset



**Replication**:
	•	Partitions can be replicated across multiple brokers for fault tolerance



**Example Use Cases
**
Topic: "user-clicks"
	•	Stores all user click events from a website

Topic: "order-events"
	•	Stores all order creation, update, and cancellation events

Topic: "sensor-data"
	•	Stores IoT sensor readings



**How It Works
**	1.	Producers write messages to topics
	2.	Messages are distributed across partitions
	3.	Consumers read messages from topics
	4.	Multiple consumers can read from the same topic independently



**Topic Structure 
**
Topic: "orders"
  - Partition 0: [msg1, msg2, msg3, ...]
  - Partition 1: [msg4, msg5, msg6, ...]
  - Partition 2: [msg7, msg8, msg9, ...]

Topics enable decoupling of data producers and consumers, making Kafka ideal for event streaming and real-time data pipelines.


### Kafka Topic and Partitions
- Named container for similar events.
- Student topic will have student related data.
- Similar to tables in database.
- Live inside a broker.
- Produces will produce a message into the topic(ultimately to a partion of that topic either using round robin or directly).
- Consumer poll continously for new messages using topic name.

### Partition
- A topic will have partitions. Eg. a topic called A has three partitions P1, P2, P3. 

### Replication Factor
- A partition is replicated by this factor and it is replicated to another broker for fault torerance.

### Partition
- A topic will have partitions. Eg. a topic called A has three partitions P1, P2, P3. 
- Actual data/message will be located in some partition of the topic.
- While creating the topic, we need to specify how much partitions to make.
- Each partition is ordered, data inside the partion is in some order, and is immutable sequence of records.
- Each message is stored into a parition with an incremental id known as its offset value.
- Order is mainted inside the parition level and not on topic level. If you need order for some data, make sure to store it on same partition.
- Partions grow and new records are produced and offset value increases.
- All the records exist in a distributed log file.

## Producing Data
Message --> Broker --> Topic --> Partition

Message can be sent with key or without key. 

A topic will have many partitions. Which partion should store the message/data? 

### Data/messages without key
- If we send data without a key, then the message will be stored based on round robin fasion among the partions. If we want strict ordering we need to store data just in a single partion.
- Messages --> a, b, c, d
- Partions --> P1, P2
- P1 will a, then P2 will get b, then P1 will get c and then P2 will get D. P1 --> a, c and P2 --> b, d
- In each partition, see that ordering is maintained.

### Data/messages with key
- If we send data with a key, we can set any key, then a partitioner will apply hashing and based on the hasing result, it will decide which parition to store this data/message.
- Same key means message will be stored in same partition.
- Data   Key
- A 	hello
- B	    hello
- C     hello

- All A, B, C will be stored in the same parition since their keys are same.
- Partition will put the data in a parition based on weather we supplied a key or not.
- Data   Key
- A 	hello
- B	    hello
- C     hello
- D	    bye

- All A, B, C will be stored in the same parition since their keys are same. But D will be put in some other partion based on its different key.

## Message
- Message will have a key(optional) and a value. 

## Ordering
- With key, ordering is maintained for the messages with the same key.
- Without key, we cannot gurantee ordering at the topic level because messages are stored in robin round fasion and consumer poll the messages from all partitions. But ordering is maintained at the partition level.
- Either use a single parition or use key for maintaing order. 

## Consumer Offset and Cosumer Groups
How cosumer consumes message?
### Consumer Offset
- Position of consumer inside a partition. 
- Which offset message is it reading. 
- Which message is the cosumer reading at the moment. 
- It represents latest message cosumer has read. 

When cosumer group reads messages from a topic, each member of the group maintains its own offset and updates it as it consumes messages. 

### Cosumer Group 
- Each cosumer belongs to a group.
- A cosumer group is a bunch consumers which has the same consumer group id. They belong to same group.
- Let us say we have three cosumers in a consumer group, a, b, c.
- We have three partitions of a topic 1, 2, 3.
- a reads 1, b reads 2, c reads 3.
- a, b, c each of them will keep a bookmark called cosumer offset, which will represent the latest message read by that cosumer in its partition. One consumer will read from one partition.

### Where is all this stored?
- A topic called __consumer_offset topic is automatically created.
- It is a built in topic that keeps track of the latest offset committed for each partition of each consumer group.
- not meanth to be read or written by client. 
- reflects the position of each consumer in each partition.
- use by kafka to maintaing reliability of consumer group and to ensure that messages are not lost or duplicated. 
