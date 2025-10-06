+++
weight = 8
title = "Mesh"
description = "A Mesh or Grid is a virtual layer of interconnected components which makes a distributed Middleware. It features supreme fault tolerance and scalability."
images = ["/diagrams/Main/Mesh.svg"]
[sitemap]
  priority = 0.8
+++

# Mesh

<figure>
<a href="/diagrams/Main/Mesh.png">
<picture>
<source srcset="/diagrams/Main/Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Mesh.png" alt="Mesh" loading="lazy" width="804" height="436" style="width:100%"/>
</picture>
</a>
</figure>

*Hive mind\.* Go decentralized\.

<ins>Known as:</ins> Mesh, Grid\.

<ins>Aspects:</ins> those of [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}})\.

<ins>Variants:</ins> *Meshes* vary greatly\. Examples include:

- [Peer\-to\-Peer Networks](https://en.wikipedia.org/wiki/Peer-to-peer),
- Leaf\-Spine Architecture / [Spine\-Leaf Architecture](https://www.geeksforgeeks.org/spine-leaf-architecture/),
- Actors,
- [Service Mesh](https://buoyant.io/service-mesh-manifesto) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\],
- [Space\-Based Architecture](https://en.wikipedia.org/wiki/Space-based_architecture) \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\.


<ins>Structure:</ins> A system of interconnected [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}) which usually make a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}})\.

<ins>Type:</ins> Implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| No single point of failure | Overhead in administration and security |
| The system is able to self\-heal | Performance is likely to suffer |
| Great scalability | The *Mesh* itself is very hard to debug |
| Available off the shelf | Unreliable communication must be accounted for in the code |

<ins>References:</ins> [Wikipedia](https://en.wikipedia.org/wiki/Network_topology#Classification) and \[[DDIA]({{< relref "../appendices/books-referenced.md#ddia" >}})\] on topology and protocols\. \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] on *Service Mesh* and *Space\-Based Architecture*\. A [long](https://buoyant.io/service-mesh-manifesto) and [short](https://www.oracle.com/cloud/cloud-native/service-mesh/what-is-a-service-mesh/) article on *Service Mesh*\.

If a system is required to survive faults, all of its components must be both [*sharded*]({{< relref "../basic-metapatterns/shards.md" >}}) and interconnected, which makes a *Mesh* – a network of interacting instances \(*nodes*\)\. In most cases the lower layer of a *shard* implements connectivity while the business logic resides in its upper layer\(s\)\. Whilst the connectivity component tends to be identical in every node of a system, the upper components may be identical – forming [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}), or different – forming [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.

Most *Meshes* support adding and removing parts of their networks dynamically, which allows for scaling up, scaling down, and fault recovery\. That is achieved through a flexible network topology, which has the chance of missing or duplicating requests, which may lead to a single action being executed by two instances of a service in parallel or by the same instance twice\. Moreover, *Mesh*\-mediated communication is likely to be slower than direct one\.

### Performance

In most \(all?\) implementations the user *application* is colocated with a *node* of the *Mesh*, thus communicating through the *Mesh* does not add an extra network hop \(which would strongly degrade performance\)\. However, that holds true only when the *Mesh node* knows the destination of the message it should send – when it has already established a communication channel towards it\. Finding a new destination may not always be easy and would often require consulting registries and sometimes waiting for the network topology to stabilize, which may involve timeouts \(like the ones you could have experienced with torrents\)\. On the other hand, no other architecture is known to seamlessly support huge networks\.

### Dependencies

*Mesh*, being a *sharded Middleware*, inherits dependencies from both of its parent metapatterns:

- As with [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}), the services that run over a *Mesh* depend both on the *Mesh’s API* and on each other \(or on a shared message format, aka [*Stamp Coupling*]({{< relref "../extension-metapatterns/shared-repository.md#inexact-stamp-coupling" >}}), or a [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) if they [use one for communication]({{< relref "../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\)\.
- As with [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}), the nodes of the *Mesh* should communicate through a backward\- and forward\-compatible protocol as there will likely be periods of time when multiple versions of the *Mesh* nodes coexist\.


### Applicability

*Mesh* <ins>is perfect</ins> for:

- *Dynamic scaling\.* Instances of services may be quickly added or removed\.
- *High availability\.* A *Mesh* is very hard to disable or kill because it both creates new instances of failed services and finds routes around failed connections\.


*Mesh* <ins>fails</ins> in:

- *Low latency domains\.* Spreading information through a *Mesh* is slow and sometimes unreliable\.
- *Security\-critical systems\.* A public *Mesh* exposes a high attack surface while the scalability of private deployments is limited by the installed hardware\.
- *Quick and dirty programming*\. The possible message duplication may cause evil bugs if you ignore the risks\.


### Relations

*Mesh*:

- Misuses [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}})\.
- Uses [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Is the base for running multiple instances of a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}), or [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Implements a distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}), [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}), or [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\.


## Variants

*Meshes* are known to vary:

### By structure

The connections in a *Mesh* may be:

- *Structured* or *pre\-defined* – the *Mesh* is pre\-designed and hard\-wired\. This kind of topology provides redundancy but not scalability\.
- *Unstructured* or *ad\-hoc* – *nodes* can be added and removed at runtime, causing restructuring of the *Mesh*\.


### By connectivity

Each *node* is:

- Connected to all other nodes – a *fully connected Mesh*\. Such *Meshes* are limited in size because the number of interconnections grows as a square of the number of nodes\. Notwithstanding, they offer the best communication speed and delivery guarantees\.
- Connected to some other nodes\. There are many possible [*topologies*](https://en.wikipedia.org/wiki/Network_topology#Classification) with the correct choice for any given task better left to experts\.


### By the number of mesh layers

The connected *nodes* of a *Mesh* may be:

- Identical \(one\-layer *Mesh*\)\. A node behaves according to its site in the network\.
- Specialized \(multi\-layer *Mesh*\)\. Some nodes implement *trunk* \(route messages and control the topology\) while others are *leaves* \(run user applications\)\.


## Examples

### Peer\-to\-Peer Networks

<figure>
<a href="/diagrams/Variants/4/P2P.png">
<picture>
<source srcset="/diagrams/Variants/4/P2P.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/P2P.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/P2P.png" alt="P2P" loading="lazy" width="802" height="403" style="width:100%"/>
</picture>
</a>
</figure>

[*Peer to Peer*](https://en.wikipedia.org/wiki/Peer-to-peer) \(*P2P*\) networks are intended for massive resource sharing over unstable connections\. The *resource* in question may be data \(torrents, blockchain, [P2PTV](https://en.wikipedia.org/wiki/P2PTV)\), CPU time \([volunteer computing](https://en.wikipedia.org/wiki/Volunteer_computing), [distributed compilation](https://en.wikipedia.org/wiki/Distcc)\), or Internet access \([Tor](https://en.wikipedia.org/wiki/Tor_(network)), [I2P](https://en.wikipedia.org/wiki/I2P)\)\. In most cases it is shared over an *unstructured* \(as participants join and leave\) *2\-layer* \(there are dedicated servers that register and coordinate users\) network which is [*overlaid*](https://en.wikipedia.org/wiki/Overlay_network) on top of the Internet\. All the leaf nodes run identical narrowly specialized software \(i\.e\. either file sharing or blockchain but not both at once\) which provides the clients with access to resources of other nodes, making a kind of distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) or [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\.

Examples: torrent, onion routing \(Tor\), blockchain\.

### Leaf\-Spine Architecture, Spine\-Leaf Architecture

<figure>
<a href="/diagrams/Variants/4/Leaf-Spine.png">
<picture>
<source srcset="/diagrams/Variants/4/Leaf-Spine.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Leaf-Spine.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Leaf-Spine.png" alt="Leaf-Spine" loading="lazy" width="603" height="361" style="width:81%"/>
</picture>
</a>
</figure>

This [datacenter network architecture](https://www.geeksforgeeks.org/spine-leaf-architecture/) is a rare example of a *structured fully connected Mesh*\. It consists of client\-facing \(*leaf*\) and internal \(*spine*\) switches\. Each *leaf* is connected to every *spine*, allowing for very high bandwidth \(by distributing the traffic over multiple routes\) that is almost insensitive to failures of individual hardware as there are always many parallel connections\.

### Actors

<figure>
<a href="/diagrams/Variants/4/Actors.png">
<picture>
<source srcset="/diagrams/Variants/4/Actors.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Actors.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Actors.png" alt="Actors" loading="lazy" width="862" height="304" style="width:100%"/>
</picture>
</a>
</figure>

A system of *Actors* may be classified as a *fully connected Mesh* with the actors’ message queues being the nodes of the *Mesh*\. Any actor can post messages to the queue of any other actor it knows about, as all the actors share a virtual namespace or physical address space\.

### Service Mesh

<figure>
<a href="/diagrams/Variants/4/Service%20Mesh.png">
<picture>
<source srcset="/diagrams/Variants/4/Service%20Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Service%20Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Service%20Mesh.png" alt="Service Mesh" loading="lazy" width="822" height="549" style="width:100%"/>
</picture>
</a>
</figure>

A [*Service Mesh*](https://buoyant.io/service-mesh-manifesto) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}), [MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] is a distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) for running [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}})\. It is a 2\-layer *Mesh* which contains one or few management nodes \(*control plane*\) and many user nodes \(*data plane*\)\. Each data plane node colocates:

- A *mesh engine node* that deals with connectivity,
- One or more [*Sidecars*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] \([*Proxies*]({{< relref "../extension-metapatterns/proxy.md" >}}) where the support of *cross\-cutting concerns* – the identical code for use by every service, e\.g\. logging or encryption – resides\),
- A user *application* \(*microservice*\) that differs from node to node\.


The control plane \(re\-\)starts, updates, scales, and collects statistics from the nodes of the data plane\.

*Service Mesh* addresses some of the weaknesses of naive [*Services*]({{< relref "../basic-metapatterns/services.md" >}}): it provides tools for centralized management and allows for virtual [sharing]({{< relref "../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}) \(through creating physical copies\) of libraries to be accessed by all the service instances\. It also takes care of scaling and load balancing\. 

Ready\-to\-use *Service Mesh* frameworks are popular with the *Microservices* architecture\.

### Space\-Based Architecture

<figure>
<a href="/diagrams/Variants/4/Space-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Variants/4/Space-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Space-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Space-Based%20Architecture.png" alt="Space-Based Architecture" loading="lazy" width="743" height="483" style="width:100%"/>
</picture>
</a>
</figure>

[*Space\-Based Architecture*](https://en.wikipedia.org/wiki/Space-based_architecture) \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] is a kind of [*Service Mesh*]({{< relref "#service-mesh" >}}) with an integrated [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) \(a [*tuple space*](https://en.wikipedia.org/wiki/Tuple_space) – shared dictionary – called [*Data Grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\) and an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \(called *Processing Grid*\)\. The user services are called *Processing Units*\. They may be identical \(making [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}})\) or different \(resulting in [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\)\. This architecture is used for:

- Highly scalable systems with relatively small datasets in which case the entire database contents are replicated in the memory of each node\. This works around the throughput and latency limits of a normal database\.
- Huge datasets, with each node owning a part of the total data\. This hacks around the storage capacity and latency limits of a database which may even be kept out of the loop, leaving the *Mesh* as the only data storage\.


There are multiple instances of the same data in *Processing Units*\. Any change to the data in one unit must propagate to other units\. That can be done in several ways:

- Asynchronously, causing conflicts if the same data is changed elsewhere simultaneously\. 
- Synchronously, waiting for the propagation results and conflict resolution – a kind of distributed transaction which has poor latency\.
- The unit takes write ownership of the data before the write\. That is not good for latency as well, but it may be a good choice for an evenly distributed load if the *Mesh* engine provides temporary locality of requests, i\.e\. it forwards requests that touch the same data to the same node\.


The choice of the strategy depends on your domain\.

The in\-memory data in the nodes is usually loaded from a *Persistent Database* on initialization of the system and any change to the data is replicated asynchronously back to the *Persistent Database*, which serves as a means of fault recovery in the unlikely case the entire *Mesh* goes down\.

## Summary

*Mesh* is a layer of intercommunicating instances of an infrastructure component that makes a foundation for running custom services in a distributed environment\. This architecture is famous for its scalability and fault tolerance but is too complex to implement in\-house and may incur performance, administration and development overhead\.