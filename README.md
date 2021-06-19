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
- "Key" part of Kafka object is probably not a unique identifier for the event. It's more likely an identifier of some entity in the system, like a User or an Order or a particular connected device.
- "Keys" play crucial part in Kafka because it helps Kafka achieve parallelism and data locality and things like that.

## Event:
- Event is just a thing that has happened combined with a description of what happened. For e.g.
    * User Interaction (somebody mousing/clicking over a certain link on a screen)
    * A microservice completes some unit of work and wants to put that record of work somewhere.
- Event is a combination of "Notification" and "State".
    * "State" of an event is usually fairly very small and is normally represented in some structured format (JSON).
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