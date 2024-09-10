---
resource:
---

# Overview
- Event-driven architecture is a [software architecture](https://www.redhat.com/en/topics/cloud-native-apps/what-is-an-application-architecture) and model for application design.
- An event-driven system is designed to capture, communicate, and process **events between decoupled services**. 
- This means that systems can remain asynchronous while still sharing information and accomplishing tasks. 
- Many modern application designs are event-driven, such as [customer engagement frameworks](https://www.redhat.com/en/solutions/customer-experience) that must utilize customer data in real time. 
- Event-driven apps can be created in any programming language because event-driven is a programming approach, not a language.
- Event-driven architecture enables **minimal coupling,** which makes it a good option for modern, distributed application architectures.

- An EDA is **loosely coupled** because 
1. event producers don’t know which event consumers are listening for an event,
2. and the event doesn’t know what the consequences are of its occurrence.
#  What's an event?
- An event acts as a record of  any significant occurrence or change in state for system hardware or software.
- An event is not the same as an event notification, which is a message or notification sent by the system to notify another part of the system that an event has taken place.
- The source of an event can be from internal or external inputs. 
	- Events can be generated from a user, like a mouse click or keystroke, an external source, such as a sensor output,
	- or come from the system, like loading a program.
# Decoupling vs loose coupling

>**Decoupling** is the practice of eliminating or minimizing direct dependencies between separate components within a system, so that no one component must rely on another.

In the context of EDA, decoupling is achieved by making sure that the components generating events are sending event data **without a particular consumer component in mind**. This disconnection allows for components to be independent and creates a more flexible system overall.  
  
>Loose coupling is a specific form of decoupling that aims to reduce the degree of interdependence between components, but not separate them completely. In a loosely coupled system, components may interact with one another but do so in a way that doesn’t create any form of reliance.  
  
Both systems promote flexibility and independence, which in turn create systems that are both agile and scalable.
# How does event-driven architecture work?
Event-driven architecture is made up of:
1. event producers (publishers) 
2. and event consumers (subscribers). 
**An event producer detects or senses an event and represents the event as a message.**  Because of decoupling, it does not know the consumer of the event, or the outcome of an event.
After an event has been detected, **it is transmitted from the event producer to the event consumers through event channels, where an event processing platform processes the event in an asynchronous way**. Event consumers need to be informed when an event has occurred. They might process the event or may only be impacted by it.  
The event processing platform will execute the correct response to an event and send the activity downstream to the right consumers. This downstream activity is where the outcome of an event is seen.
[Apache Kafka](https://www.redhat.com/en/topics/integration/what-is-apache-kafka) is a distributed data streaming platform that is a popular **event processing platform** choice. It can handle publishing, subscribing to, storing, and processing event streams in real time. Apache Kafka supports a range of use cases where high throughput and scalability are vital, and by minimizing the need for point-to-point integrations for data sharing in certain applications, it can reduce latency to milliseconds.
# Event-driven architecture models
**Pub/sub model**  
This is a messaging infrastructure based on subscriptions to an event stream. With this model, after an event occurs, or is published, it is sent to subscribers that need to be informed.  
  
**Event streaming model**  
With an event streaming model, **events are written to a log**. Event consumers don’t subscribe to an event stream. Instead, they can read from any part of the stream and can join the stream at any time.  
There are a few different types of event streaming:
- Event stream processing uses a data streaming platform, like Apache Kafka, to ingest events and process or transform the event stream. 
- Simple event processing is when an event immediately triggers an action in the event consumer.
Complex event processing requires an event consumer to process a series of events in order to detect patterns. Event stream processing can be used to detect meaningful patterns in event streams.
# Benefits of event-driven architecture
An EDA can help organizations achieve ** ** that can improve workflows by adapting to changes and making decisions in real time. Real time situational awareness means that business decisions, whether manual or automated, can be made using all of the available data that reflects the current state of your systems.  
  
Events are captured as they occur from event sources such as [Internet of Things (IoT)](https://www.redhat.com/en/topics/internet-of-things/what-is-iot) devices, applications, and networks, allowing event producers and event consumers to share status and response information in real time.  
  
Organizations can add event-driven architecture to their systems and applications to improve the **scalability and responsiveness** of applications and access to the data and context needed for better business decisions.  
  
Event-driven architecture offers **the advantage of decoupling**, where producers and consumers of data or services do not need to communicate directly, allowing for a more **flexible and scalable system**. This in turn **simplifies the integration of new components**, **promotes fault tolerance**, and enhances the overall **efficiency** of the system

---
https://www.redhat.com/en/topics/integration/what-is-event-driven-architecture