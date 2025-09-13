+++
weight = 6
title = "Middleware"
description = "A Middleware provides system components with means of communication. It may also manage their deployment, scaling and failure recovery."
+++

# Middleware

<figure>
<a href="/Main/Middleware.png" style="outline:none">
<img src="/Main/Middleware.png" alt="Middleware" style="width:100%"/>
</a>
</figure>

*The line between disorder and order lies in logistics\.* Use a shared transport\.

<ins>Known as:</ins> \(Distributed\) Middleware\.

<ins>Aspects:</ins>

- Message Broker \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [EIP]({{< relref "../appendices/books-referenced.md#eip" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\],
- Deployment Manager \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\.


<ins>Variants:</ins> Implementations differ in many dimensions\.

The following combined patterns include *Middleware*:

- \(with [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\) Message Bus \[[EIP]({{< relref "../appendices/books-referenced.md#eip" >}})\],
- \(with [*Proxies*]({{< relref "../extension-metapatterns/proxy.md" >}})\) Service Mesh \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\],
- \(with an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\) Event Mediator \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\],
- \(with a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\) Persistent Event Log / Shared Event Store,
- \(with an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\) [Enterprise Service Bus](https://www.confluent.io/learn/enterprise-service-bus/) \(ESB\) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\.


<ins>Structure:</ins> A low\-level layer that provides connectivity\.

<ins>Type:</ins> Extension\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Separates connectivity concerns from the services that use it | May become a single point of failure |
| Transparent scaling of components | May increase latency |
| Available off the shelf | A generic *Middleware* may not fit specific communication needs |


<ins>References:</ins> \[[EIP]({{< relref "../appendices/books-referenced.md#eip" >}})\] has a lot of content on the implementation of messaging *Middleware*\. \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] features a chapter on *Middleware*\. However, those books are old\. \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] is about Kafka, but it goes too far advertising it as a [*Shared Event Store*]({{< relref "#persistent-event-log-shared-event-store" >}})\. There is also a [Wikipedia article](https://en.wikipedia.org/wiki/Middleware_(distributed_applications))\.

Extracting transport into a separate layer relieves the components that implement business logic of the need to know the addresses and statuses of each other’s instances\. An industrial\-grade, third\-party *Middleware* is likely to be more stable and provide better error recovery than anything an average company can afford to implement on its own\.

A *Middleware* may function as:

- A *Message Broker* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [EIP]({{< relref "../appendices/books-referenced.md#eip" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] which provides a unified means of communication and implements some cross\-cutting concerns like the persisting or logging of messages\.
- A *Deployment Manager* \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] which collects telemetry and manages service instances for error recovery and dynamic scaling\.


As *Middleware* is ubiquitous and does not affect business logic, it is usually omitted from structural diagrams\.

### Performance

A *Middleware* may negatively affect performance when compared to direct communication between services\. Old implementations \(star topology\) relied on a *Broker* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [EIP]({{< relref "../appendices/books-referenced.md#eip" >}})\] that used to add an extra network hop for each message and limited scalability\. Newer [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}})\-based variants avoid those drawbacks but are very complex and may have consistency issues \(according to the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem)\)\.

A more subtle drawback is that transports supported or recommended by a *Middleware* may be suboptimal for some of the interactions in your system, causing programmers to hack around the limitations or build higher\-level protocols on top of your *Middleware*\. Both cases can be ameliorated by adding means for direct communication between the services to bypass the *Middleware* or by using multiple specialized kinds of *Middleware*\. However, that adds to the complexity of the system – the very issue the *Middleware* promised to help with\.

### Dependencies

Each service depends both on the *Middleware* and on the API of every service it communicates with\.

<figure>
<a href="/Dependencies/Middleware.png" style="outline:none">
<img src="/Dependencies/Middleware.png" alt="Middleware" style="width:100%"/>
</a>
</figure>

You may decide to use an [*Anticorruption Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] over your *Middleware* just in case you may need to change its vendor in the future\.

### Applicability

*Middleware* <ins>helps</ins>:

- *Multi\-component systems\.* When a service has several instances deployed which need to be accessed, its clients must know the addresses of \(or have channels to\) its instances and use an algorithm for instance selection\. As the number of services grows, so does the amount of information about the others that each of them needs to track\. Even worse, services sometimes crash or are being redeployed, requiring complicated algorithms that queue messages to deliver them in order once the service returns to life\. It makes all the sense to use a dedicated component that takes care of all of that\.
- *Dynamic scaling\.* It is good to have a single component that manages routes for interservice messaging to account for newly deployed \(or destroyed\) service instances\.
- [*Blue\-green deployment*](https://martinfowler.com/bliki/BlueGreenDeployment.html)*,* [*canary release*](https://martinfowler.com/bliki/CanaryRelease.html)*, or* [*dark launching*](https://martinfowler.com/bliki/DarkLaunching.html) *\(traffic mirroring\)*\. It is easier to switch, whether fully or in part, to a new version of a service when the communication is centralized\.
- *System stability\.* Most implementations of *Middleware* guarantee message delivery with unstable networks and failing components\. Many persist messages to be able to recover from failures of the *Middleware* itself\.


*Middleware* <ins>hurts</ins>:

- *Critical real\-time paths\.* An extra layer of message processing is bad for latency\. Such messages may need to bypass the *Middleware*\.


### Relations

<figure>
<a href="/Relations/Middleware.png" style="outline:none">
<img src="/Relations/Middleware.png" alt="Middleware" style="width:100%"/>
</a>
</figure>

*Middleware*:

- Extends [*Services*]({{< relref "../basic-metapatterns/services.md" >}}), [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), or in rare cases, [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Can make a [*Bus of Buses*]({{< relref "../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}}) \([*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\) or be merged with other *extension metapatterns* into several kinds of [*Combined Components*]({{< relref "../extension-metapatterns/combined-component.md" >}})\.
- Is closely related to [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\. A persistent *Middleware* employs a *Shared Repository*\.
- Is usually implemented by a [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) or [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) \(which is often based on a *Mesh*\)\.


## Variants by functionality

There are several dimensions of freedom with a *Middleware*, some of them may be configurable:

### By addressing \(channels, actors, pub/sub, gossip\)

Systems vary in the way their components address each other:

- *Channels* \(one to one\) – components which need to communicate establish a message channel between them\. Once created, the channel accepts messages or a data stream on its input end and transparently delivers them to its output end\. Examples: sockets, Unix pipes, and private chats\.
- *Actors* \(many to one\) – each component has a public mailbox\. Any other component that knows its address or name may push a message into it\. Examples: e\-mail\.
- *Publish/subscribe* \(one to many\) – a component can publish events to topics\. Other components subscribe to those topics and one or all of the subscribers receive copies of a published event\. Examples: IP multicast and subscriptions for e\-mail notifications\.
- [*Gossip*](https://highscalability.com/gossip-protocol-explained/) \(many to many\) – an instance of a component periodically synchronizes its version of the system's state with random other instances, which further spread the updates\. As time passes, more and more participants become aware of the changes in the system, leading to eventual consistency of the [*replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) of the system's state\.


### By flow \(notifications, request/confirm, RPC\)

Control flow may take one or more of the following approaches:

- *Notifications* – a component sends a message about an event that occurred and does not really care about the further consequences or wait for a response\.
- *Request/confirm* – a component sends a message which requests that another component does something and sends back the result\. The sender may execute other tasks meanwhile\. A request usually includes a unique id that will be added to the confirmation for the *Middleware* to know which of the requests in progress the confirmation belongs to\.
- *Remote procedure call* \(RPC\) is usually built on top of a request/confirm protocol\. The difference is that the sender blocks on the *Middleware* while waiting for the confirmation, thus to the application code the whole process of sending the request and waiting for the confirmation looks like a single method call\.


### By delivery guarantee

If the transport \(network\) or the destination fails, a message may not be processed, or may be processed twice because of retries\. A *Middleware* may [promise to deliver messages](https://blog.bytebytego.com/p/at-most-once-at-least-once-exactly):

- *Exactly once*\. This is the slowest case which is [implemented through distributed transactions](https://docs.confluent.io/kafka/design/delivery-semantics.html)\. If the network, *Middleware*, or the message handler fails, there is no side effect, and the whole process of delivering and executing the message is repeated\. The *exactly once* contract is used for financial systems where money should never disappear or duplicate during a transfer\.
- *At least once*\. On failure the message is redelivered, but the previous message could have already been processed \(if only the confirmation was lost\), thus there is a chance for a message to be processed twice\. If the message is *idempotent* \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\], meaning that it sets a value \(x = 42\) instead of incrementing or decrementing it \(x = x \+ 2\), then we can more or less safely process it multiple times \(x = 42; x = 42; x = 42;\) and use the relatively fast *exactly once* guarantee\.
- *At most once*\. If anything fails, the message is lost and never retried\. This is the fastest of the three guarantees, suitable for such monitoring applications as weather sensors – it is not too bad if a single temperature measurement disappears when you receive hundreds of them every day\.
- *At will* \(no guarantee\)\. As with the bare [UDP transport](https://en.wikipedia.org/wiki/User_Datagram_Protocol#Reliability_and_congestion_control), a message may disappear, become duplicated, or arrive out of order\. That fits real\-time streaming protocols \(video or audio calls\) where it is acceptable to skip a frame while a frame coming too late is of no use at all\. Each frame contains its sequence number, and it is up to the application to reorder and deduplicate the frames it receives\.


### By persistence

A *Middleware* with a delivery guarantee needs to store messages whose delivery has not yet been confirmed\. They may be:

- Written to a database of the *broker*\.
- Persisted in a distributed database in brokerless \([*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}})\) systems\.
- Replicated over an in\-memory *Mesh* storage \(like [*Data Grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\)\.


If the messages are stored indefinitely, the *Middleware* becomes a *Persistent* [*Event Log*](https://medium.com/sundaytech/event-sourcing-audit-logs-and-event-logs-deb8f3c54663) or even a *Shared* [*Event Store*](https://cloudnative.ly/event-driven-architectures-edas-vs-event-sourcing-c8582578e87) with the schemas of the stored events coupling the involved services \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] just like the database schema does in [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\.

### By structure \(Microkernel, Mesh, Broker\)

<figure>
<a href="/Misc/Middleware.png" style="outline:none">
<img src="/Misc/Middleware.png" alt="Middleware" style="width:100%"/>
</a>
</figure>

A *Middleware* may be:

- Implemented by an underlying operating or virtualization system \(see [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\)\.
- Run as a [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) of identical modules co\-deployed with the distributed components as [*Sidecars*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}})\.
- Rely on a single *broker* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [EIP]({{< relref "../appendices/books-referenced.md#eip" >}})\] for coordination\.


The last configuration is simpler but features a single point of failure unless multiple instances of the broker are deployed and kept synchronized\.

## Examples of merging Middleware and other metapatterns

There are several patterns which extend *Middleware* with other functions: 

### Message Bus

<figure>
<a href="/Variants/2/Message%20Bus.png" style="outline:none">
<img src="/Variants/2/Message%20Bus.png" alt="Message Bus" style="width:100%"/>
</a>
</figure>

A *Message Bus* \[[EIP]({{< relref "../appendices/books-referenced.md#eip" >}})\] employs one or more [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per service to let the services intercommunicate even if they differ in protocols\. That helps to integrate legacy services without much change to their code but it degrades overall performance as up to two protocol translations per message are involved\.

### Service Mesh

<figure>
<a href="/Variants/1/Microservices.png" style="outline:none">
<img src="/Variants/1/Microservices.png" alt="Microservices" style="width:100%"/>
</a>
</figure>

*Service Mesh* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] is a smart [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}})\-based *Middleware* that manages service instances and employs at least one co\-located [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) \(called [*Sidecar*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]\) per service instance deployed\. The *Sidecars* may provide protocol translation and cover cross\-cutting concerns such as encryption or logging\. They make a good place for shared libraries\.

The internals of [*Service Mesh*]({{< relref "../implementation-metapatterns/mesh.md#service-mesh" >}}) are discussed in the [*Mesh* chapter]({{< relref "../implementation-metapatterns/mesh.md" >}})\.

### Event Mediator

<figure>
<a href="/Variants/2/Event%20Mediator.png" style="outline:none">
<img src="/Variants/2/Event%20Mediator.png" alt="Event Mediator" style="width:100%"/>
</a>
</figure>

*Event Mediator* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], which pervades both [*Event\-Driven Architecture*s]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) and [*Nanoservices*]({{< relref "../basic-metapatterns/pipeline.md#function-as-a-service-faas-nanoservices-pipelined" >}}), melds a *Middleware* \(used for delivery of messages\) and an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \(that coordinates high\-level use cases\)\. A message arrives to a service and is responded to without any explicit component on the service’s side – it appears *out of thin Middleware* which implements the entire integration logic\.

Slightly more details on the *Event Mediator* are [provided in the *Orchestrator* chapter]({{< relref "../extension-metapatterns/orchestrator.md#event-mediator" >}})\.

### Persistent Event Log, Shared Event Store

<figure>
<a href="/Variants/2/Middleware%20-%20Shared%20Event%20Store.png" style="outline:none">
<img src="/Variants/2/Middleware%20-%20Shared%20Event%20Store.png" alt="Middleware - Shared Event Store" style="width:100%"/>
</a>
</figure>

When a *Middleware* persists messages, it takes on the function \(and drawbacks\) of a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\. A *Persistent Event Log* allows to replay incoming events for a given service \(to help a debug session or fix data corrupted by a bug\) while a *Shared Event Store* also captures changes of the internal states of the services \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\], [replacing their private databases](https://cloudnative.ly/event-driven-architectures-edas-vs-event-sourcing-c8582578e87)\. However, with either approach, changing an event field impacts all the services that use the event and may involve rewriting the entire event log \(system’s history\) \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\]\.

This pattern is detailed in the [*Combined Component* chapter]({{< relref "../extension-metapatterns/combined-component.md#persistent-event-log-shared-event-store" >}})\.

### Enterprise Service Bus \(ESB\)

<figure>
<a href="/Variants/2/Enterprise%20Service%20Bus.png" style="outline:none">
<img src="/Variants/2/Enterprise%20Service%20Bus.png" alt="Enterprise Service Bus" style="width:100%"/>
</a>
</figure>

[*Enterprise Service Bus*](https://www.confluent.io/learn/enterprise-service-bus/) \(*ESB*\) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] is a mixture of *Message Bus* and *Event Mediator*\. A *ESB* blends a *Middleware* and an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and adds an [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per service as a topping\. It emerged to connect components that originated in incompatible networks of organizations that had been acquired by a corporation\.

See the [chapter about *Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}})\.

## Evolutions

A *Middleware* is unlikely to be removed \(though it may be replaced\) once it is built into a system\. There are [few evolutions for *Middleware*]({{< relref "../appendices/evolutions/evolutions-of-a-middleware.md" >}}) because it is usually a third\-party product and thus unlikely to be modified in\-house:

- If the *Middleware* in use does not fit the preferred mode of communication between some of your services, there is the option to deploy a second, specialized *Middleware*\.


<figure>
<a href="/Evolutions/2/Middleware%20add%20Middleware.png" style="outline:none">
<img src="/Evolutions/2/Middleware%20add%20Middleware.png" alt="Middleware add Middleware" style="width:100%"/>
</a>
</figure>

- If several existing systems need to be merged, that is accomplished by adding yet another layer of *Middleware*, resulting in a [*Bottom\-up Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}}) *\(Bus of Buses\)*\.


<figure>
<a href="/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.png" style="outline:none">
<img src="/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.png" alt="Middleware to Bus of Buses" style="width:100%"/>
</a>
</figure>

## Summary

A *Middleware* is a ready\-to\-use component that provides a system of services with means of communication, scalability, and error recovery\. It is very common in distributed backends\.

<nav>

| \<\< [Extension metapatterns]({{< relref "../extension-metapatterns/_index.md" >}}) | ^ [Extension metapatterns]({{< relref "../extension-metapatterns/_index.md" >}}) ^ | [Shared Repository]({{< relref "../extension-metapatterns/shared-repository.md" >}}) \>\> |
| --- | --- | --- |

</nav>



