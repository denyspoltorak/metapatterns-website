+++
weight = 5
title = "Appendix E. Evolutions."
+++

# Appendix E\. Evolutions\.

This appendix details dozens of evolutions of metapatterns to show how they connect together\. The evolutions probably have practical value through listing prerequisites, benefits, and drawbacks, but I am not sure that many readers will get through them without becoming bored to death\. The metapattern chapters in the main parts of the book include abridged versions of the sections below\.

Duplicate and similar evolutions are omitted, and I did not write any evolutions for [fragmented metapatterns]({{< relref "../part-6--analytics/real-world-inspirations-for-architectural-patterns.md#fragmented-metapatterns" >}}) as you should be able to infer them on your own after having read the book\. Furthermore, for some reason I don’t know of any evolutions for the [implementation metapatterns]({{< relref "../part-6--analytics/real-world-inspirations-for-architectural-patterns.md#implementation-metapatterns" >}})\.

## Monolith: to Shards

One of the main drawbacks of the monolithic architecture is its lack of scalability – a single running instance of your system may not be enough to serve all its clients no matter how many resources you add in\. If that is the case, you should consider [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) – *multiple instances* of a monolith\. There are following options:

- Self\-managed [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) – each instance owns a part of the system’s data and may communicate with all the other instances \(forming a [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\)\.
- *Shards* with a [*Sharding Poxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) – each instance owns a part of the system’s data and relies on the external component to choose a shard for a client\.
- A [*Pool of stateless instances*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) with a [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) and a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) – any instance can process any request, but the database limits the throughput\.
- A [*stateful instance per client*]({{< relref "../part-2--basic-metapatterns/shards.md#temporary-state-create-on-demand" >}}) with an external persistent storage – each instance owns the data related to its client and runs in a virtual environment \(i\.e\. web browser or an [actor framework]({{< relref "../part-2--basic-metapatterns/services.md#distributed-runtime-function-as-a-service-faas-including-nanoservices-backend-actors" >}})\)\.


### Implement a Mesh of self\-managed shards

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Mesh of Shards.png" alt="Monolith to Mesh of Shards" width=100%/>
</p>

<ins>Patterns</ins>: [Sharding]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\.

<ins>Goal</ins>: scale a low\-latency application with weakly coupled data\.

<ins>Prerequisite</ins>: the application’s data can be split into semi\-independent parts\.

It is possible to run several instances of an application \(*shards*\), with each instance owning a part of the data\. For example, a chat may deploy 16 servers , each responsible for a subset of users whose hashed names end in specific 4 bits \(0 to 15\)\. However, some scenarios \(renaming a user, adding a contact\) may require the shards to intercommunicate\. And the more coupled the shards become, the more complex a *mesh engine* is required to support their interactions, up to implementing distributed transactions, at that point you will have written a distributed database\.

<ins>Pros</ins>: 

- The system scales to a predefined number of instances\.
- Perfect fault tolerance if [replication]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) and error recovery are implemented\.
- Latency is kept low\.


<ins>Cons</ins>: 

- Direct communication between shards \(the mesh engine logic\) is likely to be quite complex\.
- Intershard transactions are slow and/or complicated and may corrupt data if undertested\.
- A client must know which shards own its data to benefit from low latency\. An [*Ambassador*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) [*Sharding Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) may be used on the client’s side\.


### Split data to isolated shards and add a Sharding Proxy

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Isolated Shards with Load Balancer.png" alt="Monolith to Isolated Shards with Load Balancer" width=100%/>
</p>

<ins>Patterns</ins>: [Sharding]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Sharding Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: scale an application with sliceable data\.

<ins>Prerequisite</ins>: the application’s data can be sliced into independent, self\-sufficient parts\.

If all the data a user operates on, directly or indirectly, is never accessed by other users, then multiple independent instances \(*shards*\) of the application can be deployed, each owning an instance of a database\. A special kind of *Proxy*, called *Sharding Proxy*, redirects a user request to a shard that has the user’s data\.

<ins>Pros</ins>: 

- Perfect static \(predefined number of instances\) scalability\.
- Failure of a shard does not affect users of other shards\.
- [*Canary Release*](https://martinfowler.com/bliki/CanaryRelease.html) is supported\.


<ins>Cons</ins>: 

- The *Sharding Proxy* is a single point of failure unless [*replicated*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) and increases latency unless deployed as an [*Ambassador*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\]\.


### Separate the data layer and add a load balancer

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Stateless Shards with Shared DB.png" alt="Monolith to Stateless Shards with Shared DB" width=100%/>
</p>

<ins>Patterns</ins>: [Pool]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\), [Load Balancer]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: achieve scalability with little effort\.

<ins>Prerequisite</ins>: there is persistent data of manageable size\.

As data moves into a dedicated layer, the application becomes stateless and instances of it can be created and destroyed dynamically depending on the load\. However, the *Shared Database* becomes the system’s bottleneck unless [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is used\.

<ins>Pros</ins>: 

- Easy to implement\.
- Dynamic scalability\.
- Failure of a single instance affects few users\.
- [*Canary Release*](https://martinfowler.com/bliki/CanaryRelease.html) is supported\.


<ins>Cons</ins>: 

- The database limits the system’s scalability and performance\.
- The *Load Balancer* and *Shared Database* increase latency and are single points of failure\.


### Dedicate an instance to each client

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Instance per Client.png" alt="Monolith to Instance per Client" width=100%/>
</p>

<ins>Patterns</ins>: [Create on Demand]({{< relref "../part-2--basic-metapatterns/shards.md#temporary-state-create-on-demand" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), [Virtualizer]({{< relref "../part-5--implementation-metapatterns/microkernel.md#virtualizer-hypervisor-container-orchestrator-distributed-runtime" >}}) \([Microkernel]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: very low latency, dynamic scalability, and failure isolation\.

<ins>Prerequisite</ins>: each client’s data is small and independent of other clients\.

Each client gets an instance of the application which preloads their data into memory\. This way all the data is instantly accessible and a processing fault from one client never affects the other clients\. As systems tend to have thousands to millions of clients, it is inefficient to spawn a process per client\. Instead, more lightweight entities are used: a web app in a browser or an *actor* in a [distributed framework]({{< relref "../part-2--basic-metapatterns/services.md#distributed-runtime-function-as-a-service-faas-including-nanoservices-backend-actors" >}})\.

<ins>Pros</ins>: 

- Nearly perfect dynamic scalability \(limited by the persistence layer\)\.
- Good latency as everything happens in RAM\.
- Fault isolation is one of the features of distributed frameworks\.
- Frameworks are available out of the box\.


<ins>Cons</ins>: 

- Virtualization frameworks tend to introduce a performance penalty\.
- You may need to learn an uncommon technology\.
- Scalability and performance are still limited by the shared persistence layer\.


### Further steps

In most cases *sharding* does not change much inside the application, thus the common evolutions for [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) \(to [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), and [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\) remain applicable after sharding\. We’ll focus on their scalability:

- [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) can be scaled \(often to a dramatic extent\) and deployed individually, as [exemplified by the *Three\-Tier Architecture*]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md#distribution" >}})\.
- [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) allow for subdomains to scale independently with the help of [*Load Balancers*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) or a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. They also improve performance of data storage as each service uses its own database which is often chosen to best fit its distinct needs\.
- Granular scaling can apply to [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}), but in many cases that does not make much sense as pipeline components tend to be lightweight and stateless, making it easy to scale the pipeline as a whole\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Shards - Further 1.png" alt="Monolith to Shards - Further 1" width=100%/>
</p>

There are specific evolutions of [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) that deal with their drawbacks:

- [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) reimplements [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) with [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\. Its main goal is to make the data layer dynamically scalable, but the exact results are limited by the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) thus, depending on the mode of action, it can provide very high performance with no consistency guarantees for a small dataset, or reasonable performance for a huge dataset\. It blends the best features of stateful [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) and [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \(being an option for either to evolve to\) but may be quite expensive to run and lacks algorithmic support for analytical queries\.
- [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) is a mirror image of [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}), another option to implement use cases that deal with data of multiple shards without the need for the shards to intercommunicate\. Stateless *Orchestrators* scale perfectly but may corrupt the data if two of them write to an overlapping set of records\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Shards - Further 2.png" alt="Monolith to Shards - Further 2" width=100%/>
</p>

## Monolith: to Layers

Another drawback of [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) is its … er … monolithism\. The entire application exposes a single set of qualities and all its parts \(if they ever emerge\) are deployed together\. However, life awards flexibility: parts of a system may benefit from being written in varying languages and styles, deployed with different frequency and amount of testing, sometimes to specific hardware or end users’ devices\. They may need to [vary in security and scalability]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md#distribution" >}}) as well\. Enter [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) – a subdivision by the *level of abstractness*:

- Most *Monoliths* can be divided into three or four [*layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.
- It is common to see the database separated from the main application\.
- [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) \(e\.g\. [*Firewall*]({{< relref "../part-3--extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), [*Cache*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Reverse Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}})\) are common additions to the system\.
- An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) adds a layer of indirection to simplify the system’s API for its clients\.


### Divide into Layers

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Layers.png" alt="Monolith to Layers" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: let parts of the system vary in qualities, improve the structure of the code\.

<ins>Prerequisite</ins>: there is a natural way to separate the high\-level logic from the low level implementation details and dependencies\.

Most systems apply *layering* by default as it grants a lot of flexibility at very little cost\. [Common sets of layers]({{< relref "../part-2--basic-metapatterns/layers.md#examples" >}}) are: UI, tasks \(orchestration\), domain \(detailed business rules\) and infrastructure \(database and libraries\) or frontend, backend and data\.

<ins>Pros</ins>:

- It is a natural way to specialize and decouple two or three development teams\.
- The layers may vary in virtually any quality:
  - They are deployed and scaled independently\.
  - They may run on different hardware, including client devices\.
  - They may vary in programming language, paradigm and release cycle\.
- Most changes are isolated to a single layer\.
- Layering opens a way to many evolutions of the system\.
- The code [becomes easier to read]({{< relref "../part-1--foundations/modules-and-complexity.md" >}})\.


<ins>Cons</ins>: 

- Dividing an existing application into *Layers* may take some effort\.
- There is a small performance penalty\.


### Use a database

<p align="center">
<img src="/Evolutions/Monolith/Monolith add Database.png" alt="Monolith add Database" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: avoid implementing a datastore\.

<ins>Prerequisite</ins>: the system needs to query \(maybe also persist\) a large amount of data\.

A datastore is non\-trivial to implement\. While ordinary files are good for small volumes of data, as your needs grow so needs to grow your technology\. Deploy a database\.

<ins>Pros</ins>: 

- A well\-known database is sure to be more reliable than any in\-house implementation\.
- Many databases provide heavily optimized algorithms for querying data\.
- You can choose other hardware to deploy the database to\.
- Your \(now stateless\) application will be easy to scale\.


<ins>Cons</ins>: 

- Databases are complex and require fine\-tuning\.
- You cannot adapt the database engine to your evolving needs\.
- Most databases do not scale\.
- You are stepping right into [vendor lock\-in](https://en.wikipedia.org/wiki/Vendor_lock-in)\.


<ins>Further steps</ins>:

- Deploy multiple [*instances*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of your application behind a [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\.
- Continue the transition to [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) by separating the high\-level and low\-level business logic\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) improves performance of the data layer\.
- [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) passes read and write requests through dedicated services\. 
- [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is low latency and allows for dynamic scalability of the whole system, including the data layer\.
- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) will allow you to switch to another database in the future\.


### Add a Proxy

<p align="center">
<img src="/Evolutions/Monolith/Monolith add Proxy.png" alt="Monolith add Proxy" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\.

<ins>Goal</ins>: avoid implementing generic functionality\.

<ins>Prerequisite</ins>: Your system serves clients \(as opposed to [controlling hardware]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}})\)\.

A *Proxy* is placed between your system and its clients to provide generic functionality that otherwise would have to be implemented by the system\. The kinds of *Proxy* to use with [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) are: [*Firewall*]({{< relref "../part-3--extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), [*Cache*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Reverse Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and [*Adapter*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\. Multiple *Proxies* can be deployed\.

<ins>Pros</ins>: 

- You save some time \(and money\) on development\.
- A well\-known *Proxy* is likely to be more secure and reliable than an in\-house implementation\.
- You can select hardware to deploy the *Proxy* to\.


<ins>Cons</ins>: 

- Latency degrades, except for [*Response Cache*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) where it depends on frequency of requests\.
- The *Proxy* may fail, which increases the chance of failure of your system\.
- Beware of [vendor lock\-in](https://en.wikipedia.org/wiki/Vendor_lock-in)\.


<ins>Further steps</ins>:

- Another kind of *Proxy* may be added\.
- Some systems employ a *Proxy* per client, leading to [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


### Add an Orchestrator

<p align="center">
<img src="/Evolutions/Monolith/Monolith add Orchestrator.png" alt="Monolith add Orchestrator" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: provide a high\-level API for clients to improve their developer experience and performance\.

<ins>Prerequisite</ins>: the API of your system is fine\-grained and there are common use cases which repeat certain sequences of calls to your API\.

A well\-designed *Orchestrator* should provide a high\-level API which is intuitive, easy to use, and coarse\-grained to minimize the number of interactions between the system and its clients\. An old way to access the original system’s API may still be maintained for rare use cases or legacy client applications \([*open* orchestration]({{< relref "../part-3--extension-metapatterns/orchestrator.md#open-or-relaxed" >}})\)\. As a matter of fact, you program common logic on behalf of your clients\.

<ins>Pros</ins>: 

- Client applications become easier to write\. 
- Latency improves\.


<ins>Cons</ins>: 

- You get yet another moving part to design, test, deploy, and observe; and lots of meetings between development teams for a bonus\.
- The new coarse\-grained interface will likely be less powerful than the original one\.


<ins>Further steps</ins>:

- [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) use an *Orchestrator* per client type\.


### Further steps

Applying one of the evolutions discussed above does not prevent you from following another one of them, or even the same one for a second time:

- A layer can be split into two layers\.
- A database can be added\.
- Multiple kinds of *Proxies* are OK\.
- If you don’t have an *orchestration layer* yet, you may add one\.


Those were evolutions from *Layers* to [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

Another set of evolutions stems from splitting one or more *layers* into [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}):

- Splitting a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) yields [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) where requests from each kind of client are processed by a dedicated component\.
- Splitting the layer with the main business logic results in [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), possibly augmented with layers of [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}), [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}), [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.
- Splitting the *database layer* leads to [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) with specialized storages\.
- If all the layers share the domain dimension and are split along it, [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) \(or its subtype [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\) emerge\.
- If each layer is split along its own domain, the system follows [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) that is built around component reuse\.
- Finally, some domains support [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) – a tree\-like architecture where each layer takes a share of the system’s functionality\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Layers - Further 1.png" alt="Monolith to Layers - Further 1" width=100%/>
</p>

In addition,

- Distributed systems usually allow for the [scaling]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) of one or more layers\.
- A layer may employ [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) for better customizability\.
- The UI and infrastructure layers may be split and abstracted according to the rules of [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) \(or its subtype [*Separated Presentation*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md#examples--separated-presentation" >}})\)\.
- The system can often be extended with [*Scripts*]({{< relref "../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}), resulting in a kind of [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}})\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Layers - Further 2.png" alt="Monolith to Layers - Further 2" width=100%/>
</p>

## Monolith: to Services

The final major drawback of [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) is the cohesiveness of its code\. The rapid start of development begets a major obstacle for project growth: every developer needs to know the entire codebase to be productive and changes made by individual developers overlap and may break each other\. Such distress is usually solved by dividing the project into components along *subdomain boundaries* \(which tend to match [*bounded contexts*](https://martinfowler.com/bliki/BoundedContext.html) \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\]\)\. However, that requires a lot of work, and good boundaries and APIs are [hard to design](https://martinfowler.com/bliki/MonolithFirst.html)\. Thus many organizations prefer a slower iterative transition\.

- A *Monolith* can be split into [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) right away\.
- Or only the new features may be added as new services\.
- Or the weakly coupled parts of existing functionality may be separated, one at a time\.
- Some domains allow for sequential [data processing]({{< relref "../part-1--foundations/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}) best described by [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith_ Services and Pipeline.png" alt="Monolith: Services and Pipeline" width=100%/>
</p>

### Divide into Services

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Services.png" alt="Monolith to Services" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: facilitate development by multiple teams, improve the code, decouple qualities of subdomains\.

<ins>Prerequisite</ins>: there is a natural way to split the business logic into loosely coupled subdomains, and the subdomain boundaries are sure to never change in the future\.

Splitting a *Monolith* into *Services* by subdomain [is risky in the early stages of a project](https://martinfowler.com/bliki/MonolithFirst.html) while the domain understanding is evolving \([in\-process *modules*]({{< relref "../part-2--basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) are less risky but provide fewer benefits\)\. However, this is the way to go as soon as the codebase becomes unwieldy due to its size\.

<ins>Pros</ins>: 

- Supports multiple, relatively independent and specialized development teams\.
- Lowers the penalty imposed by the project’s size and complexity on the velocity of development and product quality\.
- Each team may choose the best fitting technologies for its service\.
- The services can differ in [non\-functional requirements](https://en.wikipedia.org/wiki/Non-functional_requirement)\.
- Flexible deployment and scaling\.
- A certain degree of error tolerance for asynchronous systems\.


<ins>Cons</ins>: 

- It takes lots of work to split a *Monolith*\.
- Any future changes to the overall structure of the domain will be hard to implement\.
- Sharing data between services is complicated and error\-prone\.
- System\-wide use cases are hard to understand and debug\.
- There is a moderate performance penalty for system\-wide use cases\.


### Add or split a service

<p align="center">
<img src="/Evolutions/Monolith/Monolith Split Service.png" alt="Monolith Split Service" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: [stop digging](https://en.wikipedia.org/wiki/Law_of_holes), get some work for novices who don’t know the entire project\.

<ins>Prerequisite</ins>: the new functionality you are adding or the part you are splitting is weakly coupled to the bulk of the existing *Monolith*\.

If your [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) is already hard to manage, but a new functionality is needed, you can try dedicating a separate service to the new feature\(s\)\. This way the *Monolith* does not become larger – it is even possible that you will move a part of its code to the newly established service\.

If you are not adding a new feature but need to change an old one – use the chance to make the existing *Monolith* smaller by first separating the functionality which you are going to change from its bulk\. At the very minimum this two\-step process lowers the probability of breaking something unrelated to the changes of behavior required\.

<ins>Pros</ins>: 

- The legacy code does not increase in size and complexity\.
- The new service is transferred to a dedicated team which does not need to know the legacy system\.
- The new service can be experimented with and even rewritten from scratch\.
- The likely faults of the new service don’t crash the main application\.
- The new service can be tested and deployed in isolation\.
- The new service can be scaled independently\.


<ins>Cons</ins>: 

- The new service will have a hard time sharing data or code with the main application\.
- Use cases that involve both the new service and the old application are hard to debug\.
- There is a moderate performance penalty for using the service\.


<ins>Further steps</ins>:

- [Continue disassembling](https://martinfowler.com/bliki/StranglerFigApplication.html) the *Monolith*\.


### Divide into a Pipeline

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Pipeline.png" alt="Monolith to Pipeline" width=100%/>
</p>

<ins>Patterns</ins>: [Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\)\.

<ins>Goal</ins>: decrease the complexity of the code, make it easy to experiment with the steps of data processing, distribute the task over multiple CPU cores, processors or computers\.

<ins>Prerequisite</ins>: the domain can be represented as a sequence of coarse\-grained data processing steps\.

If you can treat your application as a chain of independent steps that transform the input data, you can rely on the OS to schedule them and you can also dedicate a development team to each of the steps\. This is the default solution for a system that [processes a stream]({{< relref "../part-1--foundations/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}) of a single type of data \(video, audio, measurements\)\. It has excellent flexibility\.

<ins>Pros</ins>: 

- Nearly abolishes the influence of project size on development velocity\.
- The project’s teams become almost independent\.
- Flexible deployment and scaling\.
- Naturally supports event replay for reproducing bugs, testing or benchmarking individual components\.
- It is possible to have multiple implementations of each of the steps of data processing\.
- Does not need any manual scheduling or thread synchronization\.


<ins>Cons</ins>: 

- Latency may skyrocket\.
- As the number of supported scenarios grows, so does the number of components and *pipelines*\. Soon there’ll be nobody who understands the system as a whole\.


### Further steps

As your knowledge of the domain and your business requirements change, you may need to move some functionality between the services to keep them loosely coupled\. Sometimes you have to merge two or three services together\. So it goes\.

Systems of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) are quite often extended with special kinds of [*layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}):

- [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) takes care of deployment, intercommunication and scaling of services\.
- [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) lets services operate on and communicate through [shared data]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}})\.
- [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) are ready\-to\-use components that add generic functionality to the system\.
- [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) encapsulates use cases that involve multiple services, so that the services don’t need to know about each other\.
- Finally, there are [*Combined Components*]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}) that implement two or more of the above patterns in a single framework\. 


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Services - Further 1.png" alt="Monolith to Services - Further 1" width=100%/>
</p>

Each service, being a smaller *Monolith*, may evolve on its own\. Most of the evolutions of [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) are applicable\. The most common examples include:

- [*Scaled \(Sharded\) Service*]({{< relref "../part-2--basic-metapatterns/services.md#scaled-service" >}}) with a [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) and [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) to support high load\.
- [*Layered Service*]({{< relref "../part-2--basic-metapatterns/services.md#layered-service" >}}) to improve the code structure and decouple deployment of parts of a service\.
- [*Cell*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) \(*Service of Services*\) to involve multiple teams and technologies within a single subdomain\.
- [*Hexagonal Service*]({{< relref "../part-2--basic-metapatterns/services.md#hexagonal-service" >}}) to escape vendor lock\-in\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Services - Further 2.png" alt="Monolith to Services - Further 2" width=100%/>
</p>

## Monolith: to Plugins

The last group of evolutions which we review does not really change the monolithic nature of the application\. Instead, its goal is to improve the *customizability* of the [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}):

- Vanilla [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) is the most direct approach which relies on replaceable bits of logic\.
- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) is a subtype of *Plugins* which is all about isolating the main code from any third\-party components it uses\.
- [*Scripts*]({{< relref "../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) is a kind of [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}}) – yet another subtype of *Plugins* – which gives users of the system full control over its behavior\.


### Support plugins

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Plugins.png" alt="Monolith to Plugins" width=100%/>
</p>

<ins>Patterns</ins>: [Plugins]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\.

<ins>Goal</ins>: simplify the customization of the application’s behavior\.

<ins>Prerequisite</ins>: several aspects need to vary from customer to customer\.

*Plugins* create points of access to the system that allow engineers to collect data and govern select aspects of the system’s behavior without having to learn the system’s implementation\.

<ins>Pros</ins>: 

- The system can be modified by internal and external programmers who don’t know its internal details\.
- Customized versions become much easier to release and support\.


<ins>Cons</ins>: 

- Extensive changes may be required to expose the tunable aspects of the system\.
- Testability becomes poor because of the number of possible variants\.
- Performance is likely to degrade\.


### Isolate dependencies with Hexagonal Architecture

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Hexagonal.png" alt="Monolith to Hexagonal" width=100%/>
</p>

<ins>Patterns</ins>: [Hexagonal Architecture]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) \([Plugins]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\)\.

<ins>Goal</ins>: isolate the business logic from external dependencies\.

<ins>Prerequisite</ins>: there are third\-party or unstable components in the system\.

The main business logic will communicate with all the external components through APIs or SPIs defined in the terms of the business logic\. This way it will not depend on anything at all and any component will be replaceable with another implementation or a [stub/mock](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub)\.

<ins>Pros</ins>: 

- Vendor lock\-in is ruled out\.
- A component may be replaced through to the very end of the system’s life cycle\.
- Stubs and mocks are supported for testing and local or early development\.
- It is possible to provide multiple implementations of a component\.


<ins>Cons</ins>: 

- Some extra effort is required to define and use the interfaces\.
- There is performance degradation, mostly due to lost optimization opportunities\.


### Add an Interpreter \(support Scripts\)

<p align="center">
<img src="/Evolutions/Monolith/Monolith to Interpreter.png" alt="Monolith to Interpreter" width=100%/>
</p>

<ins>Patterns</ins>: [Scripts aka Interpreter]({{< relref "../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) \([Microkernel]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}}) \([Plugins]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\)\)\.

<ins>Goal</ins>: allow the system’s users to implement their own business logic\.

<ins>Prerequisite</ins>: the domain is representable in high\-level terms\.

*Interpreter* lets the users develop high\-level business logic from scratch by programming interactions of pre\-defined building blocks which are implemented in the core of the system\. That provides unparalleled flexibility at the cost of performance and design complexity\.

<ins>Pros</ins>: 

- Perfect flexibility and customizability for every user\.
- The high\-level business logic is written in high\-level terms, making it fast to develop and easy to grasp\.


<ins>Cons</ins>: 

- Requires much effort to design correctly\.
- There may be a heavy performance penalty if the API is too fine\-grained\.
- Testability may be an issue\.


## Shards: share data

One issue peculiar to [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) is that of coordinating the instances deployed, especially if their data become coupled\. The most direct solution is to let the instances operate a component that wraps the shared data:

- If the whole dataset needs to be shared, it can be split into a [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) layer\. 
- If data collisions are tolerated, [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) promises low latency and dynamic scalability\.
- If a part of the system’s data becomes coupled, only that part can be moved to a *Shared Repository*, causing each instance to manage two data stores: [private and shared]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#private-and-shared-databases" >}})\.
- Another option is to split out a [*service*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) to own the coupled data and always deploy it as a single instance\. The remaining parts of the system become coupled to that service, not each other\.


### Move all the data to a Shared Repository

<p align="center">
<img src="/Evolutions/Shards/Shards to Shared DB.png" alt="Shards to Shared DB" width=100%/>
</p>

<ins>Patterns</ins>: [Pool]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\), [Load Balancer]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: don’t struggle against the coupling of the shards, keep it simple and stupid\.

<ins>Prerequisite</ins>: the system is not under pressure for data size or latency \(which can be addressed by the further evolutions\)\.

In case a shard needs to access data owned by any other shard, the prerequisite of the independence of shards starts to fall apart\. Grab all the data of all the shards and push it into a *Shared Database*, if you can \(there may be too much data or the database access may be too slow\)\. As all the shards become identical, you’ll likely add a *Load Balancer*\.

<ins>Pros</ins>: 

- You can choose one of the many specialized databases available\.
- The stateless instances of the main application become dynamically scalable\.
- Failure of a single instance affects few users\.
- [*Canary Release*](https://martinfowler.com/bliki/CanaryRelease.html) is supported\.


<ins>Cons</ins>: 

- The database limits the system’s scalability and performance\.
- The *Load Balancer* and *Shared Database* increase latency and are single points of failure\.


<ins>Further steps</ins>:

- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) will let you change your database in the future\.
- [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) decreases latency by co\-locating subsets of the data and your application\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) uses multiple specialized databases, often by separating commands and queries\. That may greatly relieve the primary \(write\) database\.
- [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) goes even further by processing read and write requests with dedicated *services*\. 


### Use Space\-Based Architecture

<p align="center">
<img src="/Evolutions/Shards/Shards to Space-Based Architecture.png" alt="Shards to Space-Based Architecture" width=100%/>
</p>

<ins>Patterns</ins>: [Space\-Based Architecture]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) \([Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}), [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\), [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: don’t struggle against the coupling between the shards, maintain high performance\.

<ins>Prerequisite</ins>: data collisions are acceptable\.

*Space\-Based Architecture* is a *Mesh* of nodes which consist of the application and a cached subset of the system’s data\. A node broadcasts any changes to its data to other nodes and it may request any data that it needs from the other nodes\. Collectively, the nodes of the *Mesh* keep the whole data cached in memory\.

Though *Space\-Based Architecture* may provide multiple modes of action, including [single write / multiple read]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) replicas, it is most efficient when there is no write synchronization between its nodes, meaning that data consistency is sacrificed for performance and scalability\.

<ins>Pros</ins>: 

- Unlimited dynamic scalability\.
- Off\-the\-shelf solutions are available\.
- Failure of a single instance affects few users\.


<ins>Cons</ins>: 

- Choose one: data collisions or mediocre performance\.
- Low latency is supported only for datasets that fit in memory of a single node\.
- High operational cost because the nodes exchange huge amounts of data\.
- No support for analytical queries\.


### Use a Shared Repository for a coupled subset of the data

<p align="center">
<img src="/Evolutions/Shards/Shards add Shared DB.png" alt="Shards add Shared DB" width=100%/>
</p>

<ins>Patterns</ins>: [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Private and Shared Databases]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#private-and-shared-databases" >}}) \([Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: solve the coupling between shards without losing performance\.

<ins>Prerequisite</ins>: the shards are coupled through a small subset of data\.

If a subset of the data is accessed by all the shards, that subset can be moved to a dedicated database, which is likely to be fast if only because it is small\. Using a distributed database that keeps its data synchronized on all the shards may be even faster\.

This approach resembles [*Shared Kernel*](https://ddd-practitioners.com/home/glossary/bounded-context/bounded-context-relationship/shared-kernel/) \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\]\.

<ins>Pros</ins>: 

- You can choose one of the many specialized databases available\.


<ins>Cons</ins>: 

- The *Shared Database* increases latency and is the single point of failure\.


### Split a service with the coupled data

<p align="center">
<img src="/Evolutions/Shards/Shards split Shared Service.png" alt="Shards split Shared Service" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\.

<ins>Goal</ins>: solve the coupling between the shards in an honorable way\.

<ins>Prerequisite</ins>: the part of the domain which causes coupling between the shards is weakly coupled to the remaining domain\.

If a part of the domain is too cohesive to be sharded, we can often move it from the main application into a dedicated service\. That way the main application remains sharded while the new service exists as a single instance\. In rare cases there is a chance to re\-shard the new service with a sharding key which is different from the one used for sharding the main application\.

This approach resembles [*Shared Kernel*](https://ddd-practitioners.com/home/glossary/bounded-context/bounded-context-relationship/shared-kernel/) \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\]\.

<ins>Pros</ins>: 

- The main code should become a little bit simpler\.
- The new service can be given to a new team\.
- The new service may choose a database that best fits its needs\.


<ins>Cons</ins>: 

- Now it’s hard to share data between the new service and the main application\.
- Scenarios that use the new service are harder to debug\.
- There is a moderate performance penalty for using the extra service\.


## Shards: share logic

Other cases are better solved by extracting the logic that manipulates multiple [*shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}):

- Splitting a [*service*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) \(as discussed [above]({{< relref "#split-a-service-with-the-coupled-data" >}})\) yields a component that represents both shared data and shared logic\.
- Adding a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) lets the shards communicate with each other without keeping direct connections\. It also may do housekeeping: error recovery, replication, and scaling\.
- A [*Sharding Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) hides the existence of the shards from clients\.
- An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) calls \(or messages\) multiple shards to serve a user request\. That relieves the shards of the need to coordinate their states and actions by themselves\.


### Add a Middleware

<p align="center">
<img src="/Evolutions/Shards/Shards add Middleware.png" alt="Shards add Middleware" width=100%/>
</p>

<ins>Patterns</ins>: [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: simplify communication between shards, their deployment, and recovery\.

<ins>Prerequisite</ins>: many shards need to exchange information, some may fail\.

A *Middleware* transports messages between shards, checks their health and recovers ones which have crashed\. It may manage data replication and deployment of software updates as well\.

<ins>Pros</ins>: 

- The shards become simpler because they don’t need to track each other\.
- There are many good third\-party implementations\.


<ins>Cons</ins>: 

- Performance may degrade\.
- Components of the *Middleware* are new points of failure\.


### Add a Sharding Proxy

<p align="center">
<img src="/Evolutions/Shards/Shards add Load Balancer.png" alt="Shards add Load Balancer" width=100%/>
</p>

<ins>Patterns</ins>: [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Sharding Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: simplify the code on the client side, hide your implementation from clients\.

<ins>Prerequisite</ins>: each client connects directly to the shard which owns their data\.

The client application may know the address of the shard which serves it and connect to it without intermediaries\. That is the fastest means of communication, but it prevents you from changing the number of shards or other details of your implementation without updating all the clients, which may be unachievable\. An intermediary may help\.

<ins>Pros</ins>: 

- Your system becomes isolated from its clients\.
- You can put generic aspects into the *Proxy* instead of implementing them in the shards\.
- *Proxies* are readily available\.


<ins>Cons</ins>: 

- The extra network hop increases latency unless you deploy the *Sharding Proxy* as an [*Ambassador*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] co\-located with every client, which brings back the issue of client software updates\.
- The *Sharding Proxy* is a single point of failure unless [*replicated*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}})\.


### Move the integration logic into an Orchestrator

<p align="center">
<img src="/Evolutions/Shards/Shards use Orchestrator.png" alt="Shards use Orchestrator" width=100%/>
</p>

<ins>Patterns</ins>: [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: isolate the shards from awareness of each other\.

<ins>Prerequisite</ins>: the shards are coupled via their high\-level logic\.

When a high\-level scenario uses multiple shards \([*Scatter\-Gather* and *MapReduce*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) are the simplest examples\), the way to follow is to extract all such scenarios into a dedicated stateless module\. That makes the shards independent of each other\.

<ins>Pros</ins>: 

- The shards don’t have to be aware of each other\.
- The high\-level logic can be written in a high\-level language by a dedicated team\.
- The high\-level logic can be deployed independently\.
- The main code should become much simpler\.


<ins>Cons</ins>: 

- Latency will increase\.
- The *Orchestrator* becomes a single point of failure with a good chance to corrupt your data\.


<ins>Further steps</ins>:

- [*Shard* or *replicate* the *Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#scaled" >}}) to support higher load and to remain online if it fails\.
- *Persist* the *Orchestrator* \(give it a dedicated database\) to make sure that it does not leave half\-committed transactions upon failure\.
- *Divide* the *Orchestrator* [into *Backends for Frontends*]({{< relref "#divide-the-orchestration-layer-into-backends-for-frontends" >}}) or a [*SOA*\-style layer]({{< relref "../part-3--extension-metapatterns/orchestrator.md#a-service-per-use-case-soa-style" >}}) if you have multiple kinds of clients or workflows, correspondingly\.


## Layers: make more layers

Not all the layered architectures are equally layered\. A [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) with a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) or database has already stepped into the realm of [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) but is far from reaping all of its benefits\. It may continue its journey in a few ways that [were earlier discussed]({{< relref "#monolith-to-layers" >}}) for *Monolith*:

- Employing a *database* \(if you don’t use one\) lets you rely on a thoroughly optimized state\-of\-the\-art subsystem for data processing and storage\.
- [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) are similarly reusable generic modules to be added at will\.
- Implementing an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) on top of your system may improve programming experience and runtime performance for your clients\.


<p align="center">
<img src="/Evolutions/Layers/Layers to Layers.png" alt="Layers to Layers" width=100%/>
</p>

It is also common to:

- Segregate the business logic into two [*layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.


### Split the business logic into two layers

<p align="center">
<img src="/Evolutions/Layers/Layers Split in Two.png" alt="Layers Split in Two" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: let parts of the business logic vary in qualities, improve the structure of the code\.

<ins>Prerequisite</ins>: the high\-level and low\-level logic are loosely coupled\.

It is often possible to split a backend into integration \([orchestration]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}})\) and domain layers\. That allows for one team to specialize in customer use cases while the other one delves deep into the domain knowledge and infrastructure\.

<ins>Pros</ins>: 

- You get an extra development team\.
- High\-level use cases may be deployed separately from business rules\.
- The layers may diverge in technologies and styles\.
- The code may [become less complex]({{< relref "../part-1--foundations/modules-and-complexity.md" >}})\.


<ins>Cons</ins>: 

- There is a small performance penalty\.
- In\-depth debugging becomes harder\.


## Layers: help large projects

The main drawback \(and benefit\) of [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) is that much or all of the business logic is kept together in one or two components\. That allows for easy debugging and fast development in the initial stages of the project but slows down and complicates work as the project [grows in size]({{< relref "../part-6--analytics/architecture-and-product-life-cycle.md#youth-development-of-features--fragmented-architectures" >}})\. The only way for a growing project to survive and continue evolving at a reasonable speed is to divide its business logic into several smaller, thus less [complex]({{< relref "../part-1--foundations/modules-and-complexity.md" >}}), components that match subdomains \(*bounded contexts* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\]\)\. There are several options for such a change whose applicability depends on the domain:

- The middle layer with the main business logic can be divided into [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) leaving the upper [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) and lower [*database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) layers intact for future evolutions\.
- Sometimes the business logic can be represented as a set of directed graphs which is known as [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}})\.
- If you are lucky, your domain is naturally a [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


### Divide the domain layer into Services

<p align="center">
<img src="/Evolutions/Layers/Layers Split Domain to Services.png" alt="Layers Split Domain to Services" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\), [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: make the code simpler and let several teams work on the project efficiently\.

<ins>Prerequisite</ins>: the low\-level business logic comprises loosely coupled subdomains\.

It is very common for a system’s domain to consist of weakly interacting *bounded contexts* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\]\. They are integrated through high\-level use cases and/or [relations in data]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}})\. For such a system it is relatively easy to divide the domain logic into *Services* while leaving the integration and data layers shared\.

<ins>Pros</ins>: 

- You get multiple specialized development teams\.
- The largest and most complex piece of code is split into several smaller components\.
- There is more flexibility with deployment and scaling\.


<ins>Cons</ins>: 

- Future changes in the overall structure of the domain will be harder to implement\.
- System\-wide use cases become somewhat harder to debug as they span over many components\.
- Performance may degrade if the *Services* and *Orchestrator* become distributed\.


<ins>Further steps</ins>:

- Continue by splitting the *Orchestrator* and *database*, resulting in [*Orchestrated Three\-Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}})\.
- Divide the *Orchestrator* \(by type of client\) into [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.
- Use multiple databases in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Scale well with [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\.


### Build an Event\-Driven Architecture over a Shared Database

<p align="center">
<img src="/Evolutions/Layers/Layers Split to Event-Driven Architecture.png" alt="Layers Split to Event-Driven Architecture" width=100%/>
</p>

<ins>Patterns</ins>: [Event\-Driven Architecture]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) \([Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\)\), [Shared Database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: untangle the code, support multiple teams, improve scalability\.

<ins>Prerequisite</ins>: use cases are sequences of loosely coupled coarse\-grained steps\.

If your system has a well\-defined workflow for processing every kind of input request, it can be divided into several [*subdomain services*]({{< relref "../part-2--basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}}), each hosting a few related steps of multiple use cases\. Each service subscribes to inputs from other services and/or system’s clients and publishes output events\.

<ins>Pros</ins>: 

- The code is divided into much smaller \(and simpler\) segments\.
- It is easy to add new steps or use cases as the structure is quite flexible\.
- You open a way to having several almost independent teams, one per service\.
- You can achieve flexible deployment and scaling as the services are stateless, but you need a *Middleware* for that\.
- The architecture naturally supports event replay as the means of reproducing bugs or testing / benchmarking individual components\.
- There is no need for explicit scheduling or thread synchronization\.


<ins>Cons</ins>: 

- The system as a whole is hard to debug\.
- You will have to live with high latency\.
- You may end up with too many components which are interconnected in too many ways\.


<ins>Further steps</ins>:

- Add a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) that supports scaling and failure recovery\.
- Split the *Shared Database* by subdomain, yielding [*Choreographed Two\-Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#choreographed-two-layered-services" >}})\.
- Scale with [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\.
- Extract the logic of use cases into an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.


### Build a Top\-Down Hierarchy

<p align="center">
<img src="/Evolutions/Layers/Layers to Hierarchy.png" alt="Layers to Hierarchy" width=100%/>
</p>

<ins>Patterns</ins>: [Top\-Down Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) \([Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\)\.

<ins>Goal</ins>: untangle the code, support multiple teams, earn fine\-grained scalability\.

<ins>Prerequisite</ins>: the domain is hierarchical\.

Splitting the lower layers into independent components with identical interfaces simplifies the managing code and allows the managed components to be deployed, developed, and run independently of each other\. Ideally, the mid\-layer components should participate in decision\-making so that the uppermost component is kept relatively simple\.

<ins>Pros</ins>: 

- Hierarchy is easy to develop and support with multiple teams\.
- Individual components are straightforward to modify or replace\.
- The components scale, deploy, and run independently\.
- The system is quite fault tolerant\.


<ins>Cons</ins>: 

- It takes time and skill to figure out good interfaces\.
- There are many components to administer\.
- Latency is suboptimal for system\-wide use cases\.


## Layers: improve performance

There are several ways to improve the performance of a [*layered system*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\. One we have [already discussed]({{< relref "#use-space-based-architecture" >}}) for [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}):

- [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) co\-locates the database and business logic and scales both dynamically\.


<p align="center">
<img src="/Evolutions/Layers/Layers to Space-Based Architecture.png" alt="Layers to Space-Based Architecture" width=100%/>
</p>

Others are new here and thus deserve more attention:

- Merging several layers improves latency by eliminating the communication overhead\.
- Scaling some of the layers may improve throughput but degrade latency\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) is the name for using multiple specialized databases\.


### Merge several layers

<p align="center">
<img src="/Evolutions/Layers/Layers Merge.png" alt="Layers Merge" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) or [Monolith]({{< relref "../part-2--basic-metapatterns/monolith.md" >}})

<ins>Goal</ins>: improve performance\.

<ins>Prerequisite</ins>: the layers share programming language, hardware setup and qualities\.

If your system’s development [is finished]({{< relref "../part-6--analytics/architecture-and-product-life-cycle.md#death-the-ultimate-release--monolith" >}}) \(no changes are expected\) and you really need that extra 5% performance improvement, then you can try merging everything back into a *Monolith* or a [*3\-Tier*]({{< relref "../part-2--basic-metapatterns/layers.md#three-tier-architecture" >}}) system \(front, back, data\)\.

<ins>Pros</ins>: 

- Enables aggressive performance optimizations\.
- The system may become easier to debug\.


<ins>Cons</ins>: 

- The code is frozen – it will be much harder to evolve\.
- Your teams lose the ability to work independently\.


<ins>Further steps</ins>:

- [*Shard*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) the entire system\.


### Scale individual layers

<p align="center">
<img src="/Evolutions/Layers/Layers_ Shard.png" alt="Layers: Shard" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), often [Load Balancer]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\)\.

<ins>Goal</ins>: scale the system\.

<ins>Prerequisite</ins>: some layers are [stateless]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) or limited to the [data of a single client]({{< relref "../part-2--basic-metapatterns/shards.md#temporary-state-create-on-demand" >}})\.

Multiple instances or layers can be created, with their number and deployment [varying from layer to layer]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md#distribution" >}})\. That may work seamlessly if each instance of the layer which receives an event which can start a use case knows the instance of the next layer to communicate to\. Otherwise you will need a *Load Balancer*\.

<ins>Pros</ins>: 

- Flexible scalability\.
- Better fault tolerance\.
- Co\-deployment with clients is possible\.


<ins>Cons</ins>: 

- More complex operations \(more parts to keep an eye on\)\.


<ins>Further steps</ins>:

- [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) scales the data layer\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) improves performance of the data layer\.


### Use multiple databases

<p align="center">
<img src="/Evolutions/Layers/Layers to Polyglot Persistence.png" alt="Layers to Polyglot Persistence" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.

<ins>Goal</ins>: optimize performance of data processing\.

<ins>Prerequisite</ins>: there are isolated use cases for or subsets of the data\.

If you have separated *commands* \(write requests\) from *queries* \(read requests\), you can serve the queries with [read\-only replicas]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of the database while the main database is reserved for the commands\.

If your types of data or data processing algorithms vary, you may deploy several [specialized databases]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#specialized-databases" >}}), each matching a subset of your needs\. That lets you achieve the best performance in widely diverging cases\.

<ins>Pros</ins>: 

- The best performance for all the use cases\.
- Specialized data processing algorithms out of the box\.
- Replication may help with error recovery\.


<ins>Cons</ins>: 

- Someone will need to learn and administer all those databases\.
- Keeping the databases consistent takes effort and the replication delay may negatively affect UX\.


<ins>Further steps</ins>:

- Serve read and write requests with different backends according to [*Command\-Query Responsibility Segregation \(CQRS\)*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.
- Separate the backend into [*services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) which match the already separated databases\.


## Layers: gain flexibility

The last group of evolutions to consider is about making the system more adaptable\. We have [already discussed]({{< relref "#monolith-to-plugins" >}}) the following evolutions for [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}):

- The behavior of the system may be modified through [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\.
- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) protects the business logic from dependencies on libraries and databases\.
- [*Scripts*]({{< relref "../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) allow for customization of the system’s logic on a per client basis\.


<p align="center">
<img src="/Evolutions/Monolith/Monolith to Layers - Further 2.png" alt="Monolith to Layers - Further 2" width=100%/>
</p>

There is also a new evolution that modifies the upper \(orchestration\) layer:

- The *orchestration layer* may be split into [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) to match the needs of several kinds of clients\.


### Divide the orchestration layer into Backends for Frontends

<p align="center">
<img src="/Evolutions/Layers/Layers to Backends for Frontends.png" alt="Layers to Backends for Frontends" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [Backends for Frontends aka BFFs]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

<ins>Goal</ins>: let each kind of client get a dedicated development team\.

<ins>Prerequisite</ins>: no high\-level logic is shared between client types\.

It is possible that your system has different kinds of users, e\.g\. buyers, sellers, and admins; or web and mobile applications\. It may be easier to support a separate integration module per kind of client than to keep all the unrelated code together in a single integration layer\.

<ins>Pros</ins>:

- Each kind of client gets a dedicated team which may choose best fitting technologies\.
- You get rid of the single large codebase of the integration layer\.


<ins>Cons</ins>: 

- There is no good way to share code between the *BFFs* \(in [naive implementation]({{< relref "../part-3--extension-metapatterns/orchestrator.md#monolithic" >}})\)\.
- There are new components to administer\.


<ins>Further steps</ins>:

- [Evolve the *BFFs*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md#evolutions" >}}) through adding a shared *layer* or [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) for common functionality\.


## Services: add or remove services

[*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) work well when each service matches a subdomain and is developed by a single team\. If those premises change, you’ll need to restructure the system:

- A new feature request may emerge outside of any of the existing subdomains, creating a new service\.
- A service may grow too large to be developed by a single team, calling for division\.
- Two services may become so strongly coupled that they fare better when merged together\.
- The entire system may need to be glued back into a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) if domain knowledge changes or interservice communication strongly degrades performance\.


### Add or split a service

<p align="center">
<img src="/Evolutions/Services/Services_ Split.png" alt="Services: Split" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: get one more team to work on the project, decrease the size of an existing service\.

<ins>Prerequisite</ins>: there is a loosely coupled \(new or existing\) subdomain that does not have a dedicated service \(yet\)\.

If you need to add a new functionality that does not naturally fit into one of the existing services, you may create a new service and maybe get a new team for it\.

If one of your services has grown too large, you should look for a way to subdivide it \(likely through a [*Cell*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) stage with a shared [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) and [*database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\) to decrease the size and, correspondingly, complexity of its code and get multiple teams to work on the resulting \(sub\)services\. However, that makes sense only if the old service is not highly cohesive – otherwise [the resulting subsystem may be more complex]({{< relref "../part-1--foundations/modules-and-complexity.md#coupling-and-cohesion" >}}) than the original service\.

<ins>Pros</ins>: 

- You get an extra development team\.
- The complexity of the code decreases \(splitting an existing service\) or does not increase \(adding a new one\)\.
- The new service is independently scalable\.


<ins>Cons</ins>: 

- You add to the operations complexity by creating a new system component and several inter\-component dependencies\.
- There is a new point of failure, which means that bugs and outages become more likely\.
- Performance \(or at least latency and cost efficiency\) of the system will deteriorate because interservice communication is slow\.
- You may have a hard time debugging use cases that involve both the old and new service\.


### Merge services

<p align="center">
<img src="/Evolutions/Services/Services_ Merge.png" alt="Services: Merge" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [Monolith]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) or [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: accept the coupling of subdomains and improve performance\.

<ins>Prerequisite</ins>: the services use compatible technologies\.

If you see that several services communicate with each other almost as intensely as they call their internal methods, they probably belong together\.

If your use cases have too high a latency or you pay too much for CPU and traffic, the issue may originate with the interservice communication and merging the services should help\. No services, no pain\.

Alternatively, as domain knowledge changes \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\], you may have to merge much of the code together only to subdivide it later along updated subdomain boundaries\. Which means you face [lots of work for no reason](https://martinfowler.com/bliki/MonolithFirst.html)\.

<ins>Pros</ins>: 

- Improved performance\.
- It becomes easy for parts of the merged code to access each other and share data\.
- The new merged *service* or *Monolith* is easier to debug than the original *Services*\.


<ins>Cons</ins>: 

- The development teams become even more interdependent\.
- There is no good way to vary qualities by subdomain\.
- You lose granular scalability by subdomain\.
- The merged codebase may be too large for comfortable development\.
- If anything fails, everything fails\.


## Services: add layers

The most common modifications to a [system of *Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) involve supplementary system\-wide *layers* which compensate for the inability of the *Services* to share anything among themselves:

- A [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) knows of all the deployed service [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}})\. It mediates communication between them and may manage their scaling and failure recovery\.
- [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] of a [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) make a virtual layer of [shared libraries]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md#sharing-functionality-or-data-among-services" >}}) for the [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) it hosts\.
- A [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) simplifies the initial phases of development and provides data consistency and [interservice communication]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}})\.
- [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) stand between the system and its clients and take care of shared aspects that otherwise would need to be implemented by every service\.
- An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) is the single place for the high\-level logic of every use case\.


Those layers may also be merged into [*Combined Components*]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}):

- [*Message Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#message-bus" >}}) is a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) that supports multiple protocols\.
- [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) combines [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(a kind of [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\) and [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.
- [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}) is an [*orchestrating*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.
- [*Shared Event Store*]({{< relref "../part-3--extension-metapatterns/combined-component.md#persistent-event-log-shared-event-store" >}}) combines [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.
- [*Enterprise Service Bus \(ESB\)*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) is an [*orchestrating*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) [*Message Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#message-bus" >}})\.
- [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) employs all the four layers: [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) and [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.


### Add a Middleware

<p align="center">
<img src="/Evolutions/Services/Services add Middleware.png" alt="Services add Middleware" width=100%/>
</p>

<ins>Patterns</ins>: [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: take care of scaling, recovery, and interservice communication without programming it\.

<ins>Prerequisite</ins>: communication between the services is uniform\.

Distributed systems may fail in a zillion ways\. You want to ruminate neither on that nor on [heisenbugs](https://en.wikipedia.org/wiki/Heisenbug)\. And you probably want to have a framework for scaling the services and restarting them after failure\. Get a third\-party *Middleware*\! Let your programmers write the business logic, not infrastructure\.

<ins>Pros</ins>: 

- You don't invest your time in infrastructure\.
- Scaling and error recovery are made easy\.


<ins>Cons</ins>: 

- There may be a performance penalty which becomes worse for uncommon patterns of communication\.
- The *Middleware* may be a single point of failure\.


<ins>Further steps</ins>:

- Use a [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) for dynamic scaling and as a way to implement [shared aspects]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md#sharing-functionality-or-data-among-services" >}})\.


### Use a Service Mesh

<p align="center">
<img src="/Variants/2/Multifunctional - Service Mesh.png" alt="Multifunctional - Service Mesh" width=100%/>
</p>

<ins>Patterns</ins>: [Service Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) \([Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}), [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\), [Sidecar]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \([Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: support dynamic scaling and interservice communication out of the box; share libraries among the services\.

<ins>Prerequisite</ins>: service instances are mostly [stateless]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}})\.

The [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) architecture boasts dynamic scaling under load thanks to its *Mesh*\-based *Middleware*\. It also allows for the services to share libraries in *Sidecars* \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] – additional containers co\-located with each service instance – to avoid duplication of generic code among the services\.

<ins>Pros</ins>: 

- Dynamic scaling and error recovery\.
- Available out of the box\.
- Provides a way to implement shared aspects \(cross\-cutting concerns\) once and use the resulting libraries in every service\.


<ins>Cons</ins>: 

- Performance degrades because of the complex distributed infrastructure\.
- You may suffer vendor lock\-in\.


### Use a Shared Repository

<p align="center">
<img src="/Evolutions/Services/Services to Shared Database.png" alt="Services to Shared Database" width=100%/>
</p>

<ins>Patterns</ins>: [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: let the services [share data]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}}), don’t invest in operating multiple databases\.

<ins>Prerequisite</ins>: the services use a uniform approach to persisting their data\.

You don’t really need every service to have a private database\. A shared one is enough in many cases\.

<ins>Pros</ins>: 

- It is easy for the services to share and synchronize data\.
- Lower operational complexity\.


<ins>Cons</ins>: 

- All the services depend on the database schema which becomes hard to alter\.
- The single database will limit performance of the system\.
- It may also become a single point of failure\.


<ins>Further steps</ins>:

- [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) scales the data layer but it is a simple key\-value store\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) is about having multiple specialized databases\.


### Add a Proxy

<p align="center">
<img src="/Evolutions/Services/Services add Proxy.png" alt="Services add Proxy" width=100%/>
</p>

<ins>Patterns</ins>: [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: use a common infrastructure component on behalf of your entire system\.

<ins>Prerequisite</ins>: the system serves its clients in a uniform way\.

Putting a generic component between the system and its clients helps the programmers concentrate on business logic rather than protocols, infrastructure or even security\.

<ins>Pros</ins>: 

- You get a choice of generic functionality without investing development time\.
- It is an additional layer that isolates your system from both clients and attackers\.


<ins>Cons</ins>: 

- There is a latency penalty caused by the extra network hop\.
- Each *Proxy* may be a single point of failure or at least needs some admin oversight\.


<ins>Further steps</ins>:

- You can always add another kind of [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\.
- If there are multiple clients that differ in their protocols, you can employ a stack of *Proxies* per client, resulting in [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


### Use an Orchestrator

<p align="center">
<img src="/Evolutions/Services/Services use Orchestrator.png" alt="Services use Orchestrator" width=100%/>
</p>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: have the high\-level logic of use cases distilled as intelligible code\.

<ins>Prerequisite</ins>: the use cases comprise sequences of high\-level steps \(which is very likely to be true for a system of [*subdomain services*]({{< relref "../part-2--basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}})\)\.

When a use case jumps over several services in a dance of [*choreography*]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}), there is no easy way to understand it as there is no single place to see it in the code\. It may be even worse with [*Pipelined*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) systems where use cases are embodied in the structure of event channels between the components\.

Extract the high\-level business logic from the choreographed services or their interconnections and put it into a dedicated component\.

<ins>Pros</ins>: 

- You are not limited in the number and complexity of use cases anymore\.
- Global use cases become much easier to debug\.
- You have a new team dedicated to the interaction with customers, freeing the other teams to study their parts of the domain or work on improvements\.
- Many changes in the high\-level logic can be implemented and deployed without touching the main services\.
- The extra layer decouples the main services from the system’s clients and from each other\.


<ins>Cons</ins>: 

- There is a performance penalty because the number of messages per use case doubles\.
- The *Orchestrator* may become a single point of failure\.
- Some flexibility is lost as the *Orchestrator* couples qualities of the services\.


<ins>Further steps</ins>:

- If there are several clients that strongly vary in workflows, you can apply [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) with an *Orchestrator* per client\.
- If the *Orchestrator* grows too large, it [can be divided]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers, services or both, the latter option resulting in a [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.
- The *Orchestrator* can be [scaled]({{< relref "../part-3--extension-metapatterns/orchestrator.md#scaled" >}}) and can have its own database\.


## Pipeline:

[*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) [inherits its set of evolutions from *Services*]({{< relref "../part-2--basic-metapatterns/services.md#evolutions" >}})\. Components can be added, split in two, merged or replaced\. Many systems employ a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) \(pub/sub or pipeline framework\), [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) \(which may be a database or file system\) or [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\.

There are a couple of *Pipeline*\-specific evolutions:

- The first service of the *Pipeline* can be promoted to [*Front Controller*]({{< relref "../part-3--extension-metapatterns/combined-component.md#front-controller" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\] which tracks status updates for every request it handles\.
- Adding an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) turns a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) into [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\. As the high\-level business logic moves to the orchestration layer, the services don’t need to interact directly, the interservice communication channels disappear and the system becomes identical to [*Orchestrated Services*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.


### Promote a service to Front Controller

<p align="center">
<img src="/Evolutions/Services/Pipeline promote Front Controller.png" alt="Pipeline promote Front Controller" width=100%/>
</p>

<ins>Patterns</ins>: [Front Controller]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \([Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}), [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\), [Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\)\.

<ins>Goal</ins>: allow for clients to query the state of their requests\.

<ins>Prerequisite</ins>: request processing steps are slow \(may depend on human action\)\.

If request processing steps require heavy calculations or manual action, clients may want to query the status of their requests and analysts may want to see bottlenecks in the *Pipeline*\. Let the first service in the *Pipeline* track the state of all the running requests by subscribing to status notifications from other services\.

<ins>Pros</ins>: 

- The state of each running request is readily available\.


<ins>Cons</ins>: 

- The first service in the pipeline depends on every other service\.


<ins>Further steps</ins>:

- The *Front Controller* may be further promoted to [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) if there is a need to support many complex scenarios\.


### Add an Orchestrator

<p align="center">
<img src="/Evolutions/Services/Pipeline use Orchestrator.png" alt="Pipeline use Orchestrator" width=100%/>
</p>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: support many use cases\.

<ins>Prerequisite</ins>: performance degradation is acceptable\.

When a [*choreographed*]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}) system is extended with more and more use cases, it is very likely to fall into integration hell where nobody understands how its components interrelate\. Extract the workflow logic into a dedicated service\.

<ins>Pros</ins>: 

- New use cases are easy to add\.
- Complex scenarios are supported\.
- The services don’t depend on each other\.
- There is a single client\-facing team, other teams are not under pressure from the business\.
- It is easier to run actions in parallel\.
- Global scenarios become debuggable\.
- The services don’t need to be redeployed when the high\-level logic changes\.


<ins>Cons</ins>: 

- The number of messages in the system doubles, thus its performance may degrade\.
- The *Orchestrator* may become a development and performance bottleneck or a single point of failure\.


<ins>Further steps</ins>:

- If there are several clients that strongly vary in workflows, you can apply [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) with an *Orchestrator* per client\.
- If the *Orchestrator* grows too large, it can be [divided]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers, services or both, the latter option resulting in a [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.
- The *Orchestrator* can be [scaled]({{< relref "../part-3--extension-metapatterns/orchestrator.md#scaled" >}}) and can have its own database\.


## Middleware:

A [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) is unlikely to be removed \(though it may be replaced\) once it is built into a system\. There are few evolutions as a *Middleware* is a third\-party product and is unlikely to be messed with:

- If the *Middleware* in use does not fit the preferred mode of communication between some of your services, there is an option to deploy a second specialized *Middleware*\.
- If several existing systems need to be merged, that is accomplished by adding yet another layer of *Middleware*, resulting in a [*Bottom\-Up Hierarchy \(Bus of Buses\)*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}})\.


### Add a secondary middleware

<p align="center">
<img src="/Evolutions/2/Middleware add Middleware.png" alt="Middleware add Middleware" width=100%/>
</p>

<ins>Patterns</ins>: [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.

<ins>Goal</ins>: support specialized communication between [scaled]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) services\.

<ins>Prerequisite</ins>: the system relies on a *Middleware* for scaling\.

If the current *Middleware* is too generic for the system’s needs, you can add another one for specialized communication\. The new *Middleware* does not manage the instances of the services\.

<ins>Pros</ins>: 

- Supports specialized communication with no need to write code for tracking the instances of services\.


<ins>Cons</ins>: 

- You still need to notify the new *Middleware* when an instance of a service is created or dies\.
- There is an extra component to administer\.


### Merge two systems by building a Bottom\-Up Hierarchy

<p align="center">
<img src="/Evolutions/2/Middleware to Bus of Buses.png" alt="Middleware to Bus of Buses" width=100%/>
</p>

<ins>Patterns</ins>: [Bottom\-up Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}}) \([Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}), [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\)\.

<ins>Goal</ins>: integrate two systems without a heavy refactoring\.

<ins>Prerequisite</ins>: both systems use *Middleware*s\.

If we cannot change the way each subsystem’s services use its *Middleware*, we should add a new *Middleware* to connect the existing *Middleware*s\.

<ins>Pros</ins>: 

- No need to touch anything in the existing services\.


<ins>Cons</ins>: 

- Performance suffers from the double conversion between protocols\.
- There is a new component to fail \(miserably\)\.


## Shared Repository:

Once a database appears, it is unlikely to go away\. I see the following evolutions to improve performance of the data layer:

- [*Shard*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) the database\.
- Use [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) for dynamic scalability\.
- Divide the data into a private database per service\.
- Deploy specialized databases \([*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


### Shard the database

<p align="center">
<img src="/Evolutions/2/Shared Database_ Shard.png" alt="Shared Database: Shard" width=100%/>
</p>

<ins>Patterns</ins>: [Sharding]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), maybe [Sharding Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\.

<ins>Goal</ins>: increase capacity and performance of the database\.

<ins>Prerequisite</ins>: the data is shardable \(consists of independent records\)\.

If your database is overloaded and the data which it contains describes independent entities \(users, companies, sales\) you can deploy multiple instances of the database with subsets of the data distributed among them\. You will need to deploy a *Sharding Proxy* or the services will have to find out which database *shard* to access by themselves, likely through hashing the record’s *primary key* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\. There is also a good chance that several smaller tables will have to be replicated to all the shards or moved to a [dedicated *Shared Database*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#private-and-shared-databases" >}}) \(resulting in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\)\.

Modern distributed databases support sharding out of the box, but an overgrown table may still impact the performance of the database\.

<ins>Pros</ins>: 

- Unlimited scalability\.
- You don’t need to change your database vendor\.
- Failure of a single database instance affects few users\.


<ins>Cons</ins>: 

- You need to manage many instances of the database\.
- The application or a custom script may have to synchronize shared tables among the instances\.
- There is no way to do joins or run aggregate functions \(such as *sum* or *count*\) over multiple shards – all that logic moves to the services that use the database\.


<ins>Further steps</ins>:

- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) or [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) describe pre\-calculating aggregates into another analytical database \([*Reporting Database*](https://martinfowler.com/bliki/ReportingDatabase.html)\)\.
- [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) may be cheaper as it scales dynamically\. However, in its default and highly performant configuration it is prone to write collisions\.


### Use Space\-Based Architecture

<p align="center">
<img src="/Evolutions/2/Shared Database to Space-Based Architecture.png" alt="Shared Database to Space-Based Architecture" width=100%/>
</p>

<ins>Patterns</ins>: [Space\-Based Architecture]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) \([Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}), [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: scale throughput of the database dynamically\.

<ins>Prerequisite</ins>: data collisions are acceptable\.

*Space\-Based Architecture* \(SBA\) duplicates contents of a persistent database to a distributed in\-memory *cache* co\-located with the services managed by the [*SBA*’s *Middleware*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}})\. That makes most database access operations very fast unless one needs to avoid write collisions\. The *Mesh Middleware* autoscales both the services and the associated data cache under load, granting nearly perfect scalability\. However, this architecture is costly because of the amount of traffic and CPU time spent on replicating data between the *Mesh nodes*\.

<ins>Pros</ins>: 

- Nearly unlimited dynamic scalability\.
- Off\-the\-shelf solutions are available\.
- Very high fault tolerance\.


<ins>Cons</ins>: 

- Choose one: data collisions or poor performance\.
- Low latency is guaranteed only when the entire dataset fits in the memory of a node\.
- High operational cost because the nodes will send each other lots of data\.
- No support for analytical queries\.


### Move the data to private databases of services

<p align="center">
<img src="/Evolutions/2/Shared Database to Services.png" alt="Shared Database to Services" width=100%/>
</p>

<ins>Patterns</ins>: [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: decouple the services or shards, remove the performance bottleneck \(*Shared Database*\)\.

<ins>Prerequisite</ins>: the domain data is weakly coupled\.

If the data clearly follows subdomains, it may be possible to subdivide it accordingly\. The services will become [*choreographed*]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}) \(or [*orchestrated*]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}) if they have an [*integration layer*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) instead of communicating through the [shared data]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}})\.

<ins>Pros</ins>: 

- The services become independent in their persistence and data processing technologies\.
- Performance of the *data layer*, which tends to limit the scalability of the system, will likely improve thanks to the use of smaller specialized databases\.


<ins>Cons</ins>: 

- The communication between the services and the synchronization of their data becomes a major issue\.
- Joins of the data from different subdomains will not be available\.
- Costs are likely to increase because of data transfer and duplication between the services\.
- You will have to administrate multiple databases\.


<ins>Further steps</ins>:

- [*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] or a [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] help a service access and join data that belongs to other services\.


### Deploy specialized databases

<p align="center">
<img src="/Evolutions/2/Shared Database to Polyglot Persistence.png" alt="Shared Database to Polyglot Persistence" width=100%/>
</p>

<ins>Patterns</ins>: [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.

<ins>Goal</ins>: improve performance and maybe fault tolerance of the data layer\.

<ins>Prerequisite</ins>: there are diverse data types or patterns of data access\.

It is very likely that you can either use [*specialized databases*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#specialized-databases" >}}) for various data types or deploy [*read\-only replicas*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of your data for analytics\.

<ins>Pros</ins>:

- You can choose one of the many specialized databases available on the market\.
- There is a good chance to significantly improve performance\.
- Replication improves fault tolerance of your data layer\.


<ins>Cons</ins>:

- It may take effort to learn the new technologies and use them efficiently\.
- Someone needs to see to the new database\(s\)\.
- You’ll likely need to work around *replication lag* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\]\.


## Proxy:

It usually makes little sense to get rid of a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) once it is integrated into a system\. Its only real drawback is a slight increase in latency for user requests which may be helped through creation of [bypass channels]({{< relref "../part-3--extension-metapatterns/proxy.md#half-proxy" >}}) between the clients and a service that needs low latency\. The other drawback of the pattern, the *Proxy*’s being a single point of failure, is countered by deploying multiple instances of the *Proxy*\.

As *Proxies* are usually third\-party products, there is very little we can change about them:

- We can add another kind of a *Proxy* on top of the existing one\.
- We can use a stack of *Proxies* per client, making [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


### Add another Proxy

<p align="center">
<img src="/Evolutions/2/Proxy add Proxy.png" alt="Proxy add Proxy" width=100%/>
</p>

<ins>Patterns</ins>: [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: avoid implementing generic functionality\.

<ins>Prerequisite</ins>: you don't have this kind of *Proxy* yet\.

A system is not limited to a single kind of *Proxies*\. As a *Proxy* represents your system without changing its function, *Proxies* are transparent, thus they are stackable\.

It often makes sense to colocate software *Proxies* or use a multifunctional *Proxy* to reduce the number of network hops between the clients and the system\. However, in a highly loaded system *Proxies* may be resource\-hungry, thus in some cases colocation strikes back\.

<ins>Pros</ins>: 

- You get another aspect of your system implemented for you\.


<ins>Cons</ins>: 

- Latency degrades\.
- More work for admins\.
- Another point of possible failure\.


### Deploy a Proxy per client type

<p align="center">
<img src="/Evolutions/2/Proxy to Backends for Frontends.png" alt="Proxy to Backends for Frontends" width=100%/>
</p>

<ins>Patterns</ins>: [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [Backends for Frontends]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

<ins>Goal</ins>: let the aspects of communication vary among kinds of clients\.

<ins>Prerequisite</ins>: your system serves several kinds of clients\.

If you have internal and external clients, or admins and users, you may want to vary the setup of *Proxies* for each kind of client, sometimes to the extent of physically separating network communication paths, so that each kind of client is treated according to its bandwidth, priority and permissions\.

<ins>Pros</ins>: 

- It is easy to set up various aspects of communication for a group of clients\.


<ins>Cons</ins>: 

- More work for admins as the *Proxies* are duplicated\.


## Orchestrator:

Employing an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) has two pitfalls:

- The system becomes slower because too much communication is involved\.
- A single *Orchestrator* may be found to be too large and rigid\.


There is one way to counter the first point and more ways to solve the second one:

- Subdivide the *Orchestrator* by the system’s subdomains, forming [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) and minimizing network communication\.
- Subdivide the *Orchestrator* by the type of client, forming [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.
- Add another [*layer*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) of orchestration\.
- Build a [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


### Subdivide to form Layered Services

<p align="center">
<img src="/Evolutions/2/Orchestrator to Layered Services.png" alt="Orchestrator to Layered Services" width=100%/>
</p>

<ins>Patterns</ins>: [Orchestrated Three\-Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) \([Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) \([Services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\)\)\.

<ins>Goal</ins>: simplify the *Orchestrator*, let the service teams own orchestration, decouple forces for the services, improve performance\.

<ins>Prerequisite</ins>: the high\-level \([orchestration]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}})\) logic is weakly coupled between the subdomains\.

If the *orchestration* logic mostly follows the subdomains, it may be possible to subdivide it accordingly\. Each service gets a part of the *Orchestrator* that mostly deals with its subdomain but may call other services when needed\. As a result, [each service orchestrates every other service]({{< relref "../part-1--foundations/arranging-communication.md#mutual-orchestration" >}})\. Still, a large part of orchestration becomes internal to the service, meaning that fewer calls over the network are involved\.

<ins>Pros</ins>:

- You subdivide the large *Orchestrator* codebase\.
- Performance is improved\.
- The services become more independent in their quality attributes\.


<ins>Cons</ins>:

- You lose the client\-facing *orchestration team* – now each service’s team will need to face its clients\.
- Service teams become interdependent \(while having equal rights\), which may result in slow development and suboptimal decisions\.
- There is no way to share code between different use cases or even take a look at all of the scenarios at once\.


<ins>Further steps</ins>:

- [*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] or a [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] help a service access and join data that belongs to other services, further reducing the need for interservice communication\.


### Subdivide to form Backends for Frontends

<p align="center">
<img src="/Evolutions/2/Orchestrator to Backends for Frontends.png" alt="Orchestrator to Backends for Frontends" width=100%/>
</p>

<ins>Patterns</ins>: [Backends for Frontends]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: simplify the *Orchestrator*, employ a team per client type, decouple qualities for clients\.

<ins>Prerequisite</ins>: clients vary in workflows and forces\.

When use cases for clients vary, it makes sense for each kind of client to have a dedicated *Orchestrator*\.

<ins>Pros</ins>: 

- The smaller *Orchestrators* are independent in qualities, technologies and teams\.
- The smaller *Orchestrators* are … well, smaller\.


<ins>Cons</ins>: 

- There is no good way to [share code]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md#sharing-functionality-or-data-among-services" >}}) between the *Orchestrators*\.


<ins>Further steps</ins>:

- You may want to add client\-specific [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) and, maybe, co\-locate them with the *Orchestrators* to avoid the extra network hop\.
- Adding another shared [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) below the ones dedicated to clients creates a place for sharing functionality among the *Orchestrators*\.
- If you are running [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) over a [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}), [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] may help to share generic code\.


### Add a layer of orchestration

<p align="center">
<img src="/Evolutions/2/Orchestrator add Orchestrator.png" alt="Orchestrator add Orchestrator" width=100%/>
</p>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: implement simple use cases quickly, while still supporting complex ones\.

<ins>Prerequisite</ins>: use cases vary in complexity\.

You may use two or three *orchestration frameworks* \(engines\) which differ in complexity\. A simple declarative tool may be enough for the majority of user requests, reverting to custom\-tailored code for rare complex cases\.

<ins>Pros</ins>:

- Simple scenarios are easy to write\.
- You retain good flexibility with hand\-written code when it is needed\.


<ins>Cons</ins>:

- Requires learning multiple technologies\.
- More components mean more failures and more administration\.
- Performance of complex requests may suffer from more indirection\.


<ins>Further steps</ins>:

- Divide one or more of the resulting *orchestration layers* to form [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}), [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}), or [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}})\.


### Form a hierarchy

<p align="center">
<img src="/Evolutions/2/Orchestrator to Hierarchy.png" alt="Orchestrator to Hierarchy" width=100%/>
</p>

<ins>Patterns</ins>: [Top\-Down Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) \([Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\)\.

<ins>Goal</ins>: simplify the *Orchestrator* and, if possible, the services\.

<ins>Prerequisite</ins>: the domain is hierarchical\.

If an *Orchestrator* becomes too complex, some domains \(e\.g\. IIoT or telecom\) encourage using a tree of *Orchestrators*, with each layer taking care of one aspect of the domain, serving the most generic functionality at the root\.

<ins>Pros</ins>: 

- Multiple specialized teams and technologies\.
- Small codebase per team\.
- Reasonable testability\.
- Some decoupling of quality attributes\.


<ins>Cons</ins>:

- Hard to debug\.
- Poor latency in global scenarios unless several layers of the *hierarchy* are colocated\.


## Combined Component:

The patterns that involve *orchestration* \([*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}), [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}), [*Enterprise Service Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\) may allow for [most of the evolutions]({{< relref "#orchestrator" >}}) of the [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) metapattern by deploying multiple versions of the *Combined Component* that differ in their orchestration logic\. There is also a special evolution:

- Replace the *Combined Component* with several specialized ones


### Divide into specialized layers

<p align="center">
<img src="/Evolutions/2/Multifunctional_ Split.png" alt="Multifunctional: Split" width=100%/>
</p>

<ins>Patterns</ins>: [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: break out of *vendor lock\-in* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\], gain flexibility\.

<ins>Prerequisite</ins>: you have lots of free time\.

If you feel that the *Combined Component* which your system relies on does not cover all your needs, or is too expensive, or unstable, then you may want to get rid of it by replacing it with generic single\-purpose tools or a homebrewed implementation that will always adapt to your circumstances\.

<ins>Pros</ins>:

- It’s free\.
- You’ll own your code\.
- Anything you write will fit your needs for as long as you spend time supporting it\.


<ins>Cons</ins>:

- Takes lots of work\.
- Performance may become worse because there will be more components on the requests’ path and also because the industry\-grade framework that you used could have been highly optimized\.


| \<\< [Appendix D\. Disclaimer\.]({{< relref "../part-7--appendices/appendix-d--disclaimer.md" >}}) | ^ [Part 7\. Appendices]({{< relref "../part-7--appendices/_index.md" >}}) ^ | [Appendix F\. Format of a metapattern\.]({{< relref "../part-7--appendices/appendix-f--format-of-a-metapattern.md" >}}) \>\> |
| --- | --- | --- |


