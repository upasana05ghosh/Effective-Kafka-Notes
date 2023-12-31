# Effective Kafka

- [Effective Kafka](#effective-kafka)
- [Chapter 1: Event Streaming Fundamentals](#chapter-1-event-streaming-fundamentals)
    - [Event-Driven Architecture (EDA)](#event-driven-architecture-eda)
    - [What is event streaming?](#what-is-event-streaming)
- [Chapter 2: Apache Kafka](#chapter-2-apache-kafka)
    - [Uses of Kafka](#uses-of-kafka)
- [Chapter 3: Architecture and Core concept](#chapter-3-architecture-and-core-concept)
    - [Total and partial order](#total-and-partial-order)
    - [Records](#records)
    - [Partition](#partition)
    - [Topics](#topics)
    - [Consumer Groups and Load balancing](#consumer-groups-and-load-balancing)
    - [Committing offsets](#committing-offsets)
- [Chapter 6: Desing Consideration](#chapter-6-desing-consideration)
  - [Roles and responsibilties](#roles-and-responsibilties)
  - [Paralleslism](#paralleslism)
  - [Idempotence and exactly-once delivery](#idempotence-and-exactly-once-delivery)

# Chapter 1: Event Streaming Fundamentals

### Event-Driven Architecture (EDA)
* is a paradigm -> reaction to events
* event -> change in state
* consist of 
  * emitters (producers)
  * consumers (subscribers)
  * channels(brokers)
* An emitter of an event is not aware of any of the event's downstream consumers. 
* Event notifications are immutable.
* Features: 
  * Coupling - loosely coupled
  * Resilience - less prone to outage (limited to individual component)
  * Consistency - maintained by enforcing the single writer principle. 

### What is event streaming?
* Event stream -> durable, totally-ordered, unbounded sequence of immutable event records, delivered at least once to its subscriber(s).

# Chapter 2: Apache Kafka

### Uses of Kafka
* Publish-subscribe
* Log aggregation ->  act as a buffer, offering an intermediate, durable database. 
* Log shipping -> real-time copying of data which can be replayed later. 
* SEDA pipeline -> Staged Event-Driven Architecture
  * event flow through a series of processing stages linked by topics. 
* CEP - Complex Event Processing 
  * extracts meaningful information and pattersn in a stream of discrete events. 
  * Used in algo sotck trading, security thread analysis, real time fraud detection.

# Chapter 3: Architecture and Core concept

* Broker Node: Host a set of append-only log files by partition. 
* Zookeeper Nodes: Act as a consistent and highly available configuration repository of cluster metadata. 
  * It's not an internal component of Kafka, but an open-source project in its own. 
* Produces: Client app responsible for appending records to Kafka topics
* Consumers: Client app that read from topics

### Total and partial order
* Total ordered set - every element has a well-defined ordering relationship with every other element in the set
  * Ex - 1-> 2 -> 3 -> 4
  * Every element has a predecessor-successor relationship with every other element. 
  * We can remove any element from the set and reinsert them back and will arrive at the same sequence.  
* Unordered set -> {Delhi, Mumbai, KolKata}
  * We don't know why Delhi should appear before Mumbai without applying any constraints. 
  * No pair of elements are comparable. 
* Partially ordered set -> Ex - set of natural no. ordered by divisibility, such that a number must appear after its divisor. 
  * {2, 3, 5, 7, 4, 6, 9, 10, 8}
  * 2 appear before 4 and 4 appear before 8 or 12.
  * Not every pair of elements needs to be comparable. 
* Casual order -> two elements may be bounded by a happened-before relationship. 
  * each element represents an event and some pairs of events have a happened-before relationship. 

### Records
* corresponds to some event of interest. 
* Attributes:
  * Key - optional non-unique key
    * represented as an array of bytes.
  * Value - payload of a record. 
  * Headers - a set of free-form key-value pairs that can optionally annotate a record.
  * Partition number - A zero-based index of the partition that the record appears in. 
  * Offset- A 64-bit interger for locating a record withing a partition. 
    * uniquely identifies it in the partition.
    * acts as a priamary key, allowing for fast, O(1) lookups.
    * strictly monotonically increasing integer
* Here, key is not a primary key. 
* Primary key - record's partition number and its offset. 

### Partition
* Published records are appeneded to a partition.
* Any pair of records in the same partition is bound by a predecessor-successor relationship (assigned by produer application)
  
### Topics
* Comprises of one or more partitions and a partition must be a part of exactly one topic

### Consumer Groups and Load balancing
* A consumer is a process or thread that attach to a kafka cluster via a client library. 
* A consumer group are effectively a load-balancing mechanism within kafka - distributing partition assignements evenly among the individual consumer instances withing the group. 
* A consumer internally maintains an offset that points to the next record in a partition.
Consumer groups also ensures availability. 
  * By peridically reading records from a topic, the consumer implicitly signals to the cluster that its in a healthy state. 
  * If the consumer fail to read again within the allowable deadline, it will be deemed faulty nad its partition will be reassigned. 

### Committing offsets
* Persisting the consumer state back to the Kafka cluster is called commiting the offset. 
* Consumer will read a record and commit the offset of the last record +1. 
 * If a new consumer takes over the topic, it will commence processing from the last commit offset
* <img width="893" alt="image" src="https://github.com/upasana05ghosh/Effective-Kafka-Notes/assets/17885669/8e6010f8-9c7f-493d-878b-dc1385c67d7c">

# Chapter 6: Desing Consideration

## Roles and responsibilties
- Who owns the topic and who's responsible for it's upkeep?
  - In single-producer-multiple-consumers, there may be a number of disaggreeing parties. 
  - Sol - Rather than feeding consumers directly, intermediate processing stages are employed to conform to consumer group's expectations. 
  - Consumer takes ownership of the conditioning stage. 

## Paralleslism
- Producer-driven partitioning
  - Responsibility of assigning records to partition lies with the producer. 
## Idempotence and exactly-once delivery
