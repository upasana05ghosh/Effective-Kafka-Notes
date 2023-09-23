# Effective Kafka

- [Effective Kafka](#effective-kafka)
- [Chapter 1: Event Streaming Fundamentals](#chapter-1-event-streaming-fundamentals)
    - [Event-Driven Architecture (EDA)](#event-driven-architecture-eda)
    - [What is event streaming?](#what-is-event-streaming)
- [Chapter 2: Apache Kafka](#chapter-2-apache-kafka)
    - [Uses of Kafka](#uses-of-kafka)

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
