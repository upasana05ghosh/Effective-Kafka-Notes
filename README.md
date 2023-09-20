# Effective-Kafka-Notes

- [Chapter 1: Event Streaming Fundamentals](#chapter-1-event-streaming-fundamentals)
    - [Event-Driven Architecture (EDA)](#event-driven-architecture-eda)
    - [What is event streaming?](#what-is-event-streaming)

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

