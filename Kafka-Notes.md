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
- Take some data from kafka cluster, make some changes to this data and put it in the cluster again. 
