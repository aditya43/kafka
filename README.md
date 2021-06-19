# Kafka
Personal Notes

## Author
Aditya Hajare ([Linkedin](https://in.linkedin.com/in/aditya-hajare)).

## Current Status
WIP (Work In Progress)!

## License
Open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

-----------

## Kafka Intro:
- It is an event streaming platform used to collect, store and process real time data streams at scale.
- It has numerous use cases like distributed logging, stream processing and pub-sub messaging.
- Kafka internally is loosely typed but externally or outside it's not loosely typed. For e.g. the programming language that we are using outside is not loosely typed, there's probably some kind of structure to the data. And so going back and forth between the way that key/value pair, that event is represented in your language's type system and the representation inside Kafka - Kafka famously calls that the process of serialization and de-serialization. Serialized format is generally JSON, Protobuf etc.
- `Key` part of Kafka object is probably not a unique identifier for the event. It's more likely an identifier of some entity in the system, like a User or an Order or a particular connected device.
- `Key` play crucial part in Kafka because it helps Kafka achieve parallelism and data locality and things like that.

## Event:
- Event is just a thing that has happened combined with a description of what happened. For e.g.
    * User Interaction (somebody mousing/clicking over a certain link on a screen)
    * A microservice completes some unit of work and wants to put that record of work somewhere.
- Event is a combination of `Notification` and `State`.
    * `State of an event is usually fairly very small and is normally represented in some structured format (JSON).
- Event in kafka is modelled as a Key/Value pair.

## Topics:
- Kafka's fundamental unit of event organization is called "Topic".
- Named container for similar events.
- Can duplicate data between topics
- They are durable logs of events.
- Logs data structure are append only. i.e. we always write at end.
    * Can only seek by offset.
    * Not indexed.
    * Logs can only be read by maybe seeking to an arbitrary offset in the log and then scanning sequential log entries.
- Kafka topics are not indexed. We can seek to an offset and scan forward from there..
- Events in logs (topics) are immutable. i.e. once something has happened, it is exceedingly dificult/impossible to make it unhappen.
- Kafka topics can be configured to messages in them to expire after they've reached a certain age.
- We can also configure if a topic is reached a specific size, delete/clean up old messages. But age/retention period is more typical thing to configure rather than size. They can be upto few seconds, years or literally up to inifinity.

## Partitions:
- Kafka is a distributed system. i.e. it is designed to operate across a number of computers.
- Kafka gives us ability to partition topics.
- Partitioning takes the single Topic Log and breaks it into multiple logs, each of which can live on a separate node in Kafka cluster.
- If a message is not having a key, then message that we write will be distributed round-robin amoungst topics partitions.
- If a message has a "Key", we use it to figure out which partition of the topic this message belongs to.
- Messages with a same "Key", always go into same partition and therefore always be in strict guarenteed order.
- `Partitions` are what take a single `topic` and break it up into many individual logs that can be hosted on different `brokers`.

## Brokers:
- Physical infrastructure standpoint, Kafka is composed of a network of machines called Brokers.
- Broker is nothing but a Computer/instance or a container running the Kafka process.
- Each broker hosts some set of Kafka partitions.
- Each broker handles read and write requests.
- Broker also manage replication of partitions.

## Replication:
- Underlying storage are susceptible to failures. So we need to copy partition data to several other brokers.
- Those copies are called `Follower Replicas`.
- Whereas the main partition is called the `Leader Replica`.
- Every partition that gets replicated has **one** `Leader Replica` and **N-1** `Follower Replicas`.
- When we produce data to the partition, we are producing it to `Leader Replica`.
- When we are writing data to a partition or reading data from partition, we are talking to `Leader Replica`.
- `Leader Replica` and `Follower Replica` work together to get the replication done so that new writes are going to `Follower Replica` and not just be on the `Leader Replica` all the time.
- This is an automated process. By default `replication` is turned `on` in Kafka.

## Producers:
- Every component of the Kafka platform that is not a `Kafka Broker` is at bottom either a `Producer`, or a `Consumer`, or both.
- `Producer` is the one who makes the decision about which `partition` to send each message to, whether to round-robin keyless messages, or to compute the destination partition by hashing the key.
- In a real sense, partitioning lives in the `Producer`.

## Consumers:
- Using the `Consumer API` is similar in principle to the `Producer`.
- We can subscribe to one or more topics.
- We can subscribe to topic(s) by passing exact topic names or regular expression that matches some topics.
- When messages are available on those topics, they come back in a collection called `ConsumerRecords`.
- `ConsumerRecords` contains individual instances of messages in the form of instances of an object called `ConsumerRecord`.
- `ConsumerRecord` object is the key value pair of actual message.
- In Kafka, scaling consumer groups is more or less automatic.
- A single instance of a consuming application will always receive the messages from all of the partitions in the topic it's subscribed to.
- Each message in each partition will come in order, messages between the partitions may not come in order.
- When we add new instance of consuming application and use same `Consumer Group Id`, that triggers an automatic rebalancing process. The Kafka cluster with client node will attempt to distribute partitions fairly between the instances of same `Consumer Group`
- Rebalancing process repeats each time we add or remove a `Consumer Group Instance`. This makes each consumer application horizontally and elastically scalable by default.
- If we had a topic with 10 partitions, we could deploy as many as 10 co `Consumer Group` instances and expect them all to participate in processing events. If we deploy an 11th instance for same `Consumer Group`, it would be idle, because there will no partition assigned to it.