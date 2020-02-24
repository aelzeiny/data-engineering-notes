
# Kafka
* Has O(1) reads & writes
* looks like 1 big distributed file
* Go to offset. Scan the file. for a few messages. Keep your offset.

### Components
* ZooKeeper - Distributed leader resolver in Hadoop ecosystem. RAFT solves a similar problem. ZooKeeper is the INTERFACE BETWEEN KAFKA BROKERS AND CONSUMERS. It saves offsets!
* Broker - This machine handles all the reads & writes. ZooKeeper elects the leader. Stateless.
* Producers - push data to brokers w/no ack
* Consumers - reading from a topic. can rewind, or skip forward to an offset. These keep track of state.

### Concepts
#### Topics
* just like topics in any queue. Defines "type"/"class" of data
* no limitation to # of topics. Kafka don't care.
#### Partitions
* Think of this as a sequential file writen to disk. The offset on a partition is the message
* if a message has a "key", it gets sorted into one partition where ordering is guarenteed.
* Ordering is only guarenteed w/i a partition.
* no limitation to # of partitions. Kafka don't care.
#### Consumer Groups
* A consumer-group is a collection of consumers going through a topic sequentially
* Partitions gets divded to the number of instances in a consumer-group
* Each instance in one consumer group gets one partition.
#### Topic Replication Factor
* replication factor <= number of available brokers
* For a given partition, only one broker can be a leader, at a time.
* Partitions have In-Sync-Replicas (ISRs) to other brokers

### Sample Workflows
#### Pub/Sub
* Producers -> Kafka Topic
* topic stores in partitions
* consumer subscribes to topic. It's offset is saved in ZooKeeper; not Broker!
* Kafka fowards message as soon as received from producer to consumer
* consumer processes
* broker receives ack for message processed. Forwards to ZooKeeper.
* consumer can rewind/skip as pleases

#### Queue Messaging/Consumer Group
* Producers -> Kafka Topic
* topic stores in partitions
* consumer group subscribes to topic. It's offset is saved in ZooKeeper; not Broker!
* Kafka fowards message as soon as received from producer to consumer-group
* each consumer in group gets & processes messages within its own sequenced partition
* If more consumers than partitions, then a consumer can be idle in a consumer-group.

### Ecosystem
* Storm Integration
* Spark Streaming:
* Streams API: Good for processing; not transforming. Native to Kafka. Simple use-cases are best.