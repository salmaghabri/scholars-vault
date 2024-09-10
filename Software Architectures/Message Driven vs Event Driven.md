> Reactive Systems rely on asynchronous  message-passing to establish a boundary between components that ensures loose coupling, isolation and location transparency . This boundary also provides the means to delegate failures as messages. Employing explicit message-passing enables load management, elasticity, and flow control by shaping and monitoring the message queues in the system and applying [back-pressure !](https://www.reactivemanifesto.org/glossary#Back-Pressure)  when necessary. Location transparent messaging as a means of communication makes it possible for the management of failure to work with the same constructs and semantics across a cluster or within a single host. [Non-blocking ](https://www.reactivemanifesto.org/glossary#Non-Blocking)  communication allows recipients to only consume [resources !](https://www.reactivemanifesto.org/glossary#Resource)  while active, leading to less system overhead.

— Jonas Bonér, Dave Farley, Roland Kuhn, and Martin Thompson, et al.
## Message vs Event

A Message is some data sent to a specific address. In Message Driven systems, each component has a unique address other components can send messages to. Each of these components, or recipients, awaits messages and reacts to them.
An Event is some data emitted from a component for anyone listening to consume.

## Sending Messages vs Emitting Events

In traditional programming models, _Component A_ invoking a method in _Component B_ was coupled in time. If _Component B_ was busy or if it was slow when handling the method invocation, then _Component A_ had to wait.

In Message Driven systems, _Component A_ produces a message indicating it must be delivered to the address of _Component B_. _Component A_ then sends the message and gets control back immediately instead of waiting for _Component B_ to complete the handling of the message. Components on a message driven system often have a queue where incoming messages can be stored in case of a load spike. Message passing is a building block for space decoupling ! 

In Event Driven systems, components announce a location where they expose their events. So a _Component A_ emitting events will use a well-known location to publish them but not know which components are consuming the events. This well-known location is implemented using an ordered queue. Sometimes the queue is indexable so consumers can keep track of the events already consumed and the events pending.
## Events _are_ messages

As discussed above, in Message Driven systems, each component send items to a fixed recipient. In Event Driven systems, on the other hand, each component produces items of data with a fixed sender and shares them with any consumer. This comparison, though, can be misleading: the term 'Message Driven' refers to a building block on a system and 'Event Driven' refers to a higher level property of a system. So, using Message Driven tools we can build an Event Driven system.

There are multiple types of messages. A categorization of messages in a [CQRS](https://developer.lightbend.com/docs/akka-guide/concepts/cqrs.html)/[ES](https://developer.lightbend.com/docs/akka-guide/concepts/event-sourcing.html) application is the following:
- **Command** — a message sent to produce a change on the state. The receiving component, if the command is valid, will update its state.
- **Event** — a message emitted by a component when its state changed. The event includes the necessary data to evolve the older state to the newer state. The event is a message sent to the publishing infrastucture where consumers may later retrieve it.
- **Query** — a message sent to a component to obtain some information from it. A Query is a special type of message because it must include a sender address so the target component can reply.
- **Reply** — the response to a query. A component handling a Query message will produce a Reply message and send it to the sender address specified in the query message.