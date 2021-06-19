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

