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




