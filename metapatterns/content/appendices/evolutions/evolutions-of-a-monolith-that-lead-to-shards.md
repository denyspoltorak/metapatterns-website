+++
weight = 1
title = "Evolutions of a Monolith that lead to Shards"
description = "Multiple stateful or stateless instances of a Monolith can be deployed to improve performance. The systems may need a Sharding Proxy or Load Balancer."
[sitemap]
  priority = 0.3
+++

# Evolutions of a Monolith that lead to Shards

One of the main drawbacks of the monolithic architecture is its lack of scalability – a single running instance of your system may not be enough to serve all its clients no matter how many resources you add in\. If that is the case, you should consider [*Shards*]({{< relref "../../basic-metapatterns/shards.md" >}}) – *multiple instances* of a monolith\. There are following options:

- Self\-managed [*Shards*]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) – each instance owns a part of the system’s data and may communicate with all the other instances \(forming a [*Mesh*]({{< relref "../../implementation-metapatterns/mesh.md" >}})\)\.
- *Shards* with a [*Sharding Proxy*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) – each instance owns a part of the system’s data and relies on the external component to choose a shard for a client\.
- A [*Pool of stateless instances*]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) with a [*Load Balancer*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) and a [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) – any instance can process any request, but the database limits the throughput\.
- A [*stateful instance per client*]({{< relref "../../basic-metapatterns/shards.md#temporary-state-create-on-demand" >}}) with an external persistent storage – each instance owns the data related to its client and runs in a virtual environment \(i\.e\. web browser or an [actor framework]({{< relref "../../basic-metapatterns/services.md#distributed-runtime-function-as-a-service-faas-including-nanoservices-backend-actors" >}})\)\.


## Implement a Mesh of self\-managed shards

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Mesh%20of%20Shards.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Mesh%20of%20Shards.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Mesh%20of%20Shards.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Mesh%20of%20Shards.png" alt="Monolith to Mesh of Shards" loading="lazy" width="1103" height="343" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Sharding]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../../basic-metapatterns/shards.md" >}})\), [Mesh]({{< relref "../../implementation-metapatterns/mesh.md" >}})\.

<ins>Goal</ins>: scale a low\-latency application with weakly coupled data\.

<ins>Prerequisite</ins>: the application’s data can be split into semi\-independent parts\.

It is possible to run several instances of an application \(*shards*\), with each instance owning a part of the data\. For example, a chat may deploy 16 servers , each responsible for a subset of users whose hashed names end in specific 4 bits \(0 to 15\)\. However, some scenarios \(renaming a user, adding a contact\) may require the shards to intercommunicate\. And the more coupled the shards become, the more complex a *mesh engine* is required to support their interactions, up to implementing distributed transactions, at that point you will have written a distributed database\.

<ins>Pros</ins>: 

- The system scales to a predefined number of instances\.
- Perfect fault tolerance if [replication]({{< relref "../../basic-metapatterns/shards.md#persistent-copy-replica" >}}) and error recovery are implemented\.
- Latency is kept low\.


<ins>Cons</ins>: 

- Direct communication between shards \(the mesh engine logic\) is likely to be quite complex\.
- Intershard transactions are slow and/or complicated and may corrupt data if undertested\.
- A client must know which shards own its data to benefit from low latency\. An [*Ambassador*]({{< relref "../../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) [*Sharding Proxy*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) may be used on the client’s side\.


## Split data to isolated shards and add a Sharding Proxy

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Isolated%20Shards%20with%20Load%20Balancer.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Isolated%20Shards%20with%20Load%20Balancer.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Isolated%20Shards%20with%20Load%20Balancer.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Isolated%20Shards%20with%20Load%20Balancer.png" alt="Monolith to Isolated Shards with Load Balancer" loading="lazy" width="1073" height="423" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Sharding]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../../basic-metapatterns/shards.md" >}})\), [Sharding Proxy]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: scale an application with sliceable data\.

<ins>Prerequisite</ins>: the application’s data can be sliced into independent, self\-sufficient parts\.

If all the data a user operates on, directly or indirectly, is never accessed by other users, then multiple independent instances \(*shards*\) of the application can be deployed, each owning an instance of a database\. A special kind of *Proxy*, called *Sharding Proxy*, redirects a user request to a shard that has the user’s data\.

<ins>Pros</ins>: 

- Perfect static \(predefined number of instances\) scalability\.
- Failure of a shard does not affect users of other shards\.
- [*Canary Release*](https://martinfowler.com/bliki/CanaryRelease.html) is supported\.


<ins>Cons</ins>: 

- The *Sharding Proxy* is a single point of failure unless [*replicated*]({{< relref "../../basic-metapatterns/shards.md#persistent-copy-replica" >}}) and increases latency unless deployed as an [*Ambassador*]({{< relref "../../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) \[[DDS]({{< relref "../../appendices/books-referenced.md#dds" >}})\]\.


## Separate the data layer and add a Load Balancer

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Stateless%20Shards%20with%20Shared%20DB.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Stateless%20Shards%20with%20Shared%20DB.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Stateless%20Shards%20with%20Shared%20DB.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Stateless%20Shards%20with%20Shared%20DB.png" alt="Monolith to Stateless Shards with Shared DB" loading="lazy" width="1083" height="423" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Pool]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \([Shards]({{< relref "../../basic-metapatterns/shards.md" >}})\), [Shared Database]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\), [Load Balancer]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: achieve scalability with little effort\.

<ins>Prerequisite</ins>: there is persistent data of manageable size\.

As data moves into a dedicated layer, the application becomes stateless and instances of it can be created and destroyed dynamically depending on the load\. However, the *Shared Database* becomes the system’s bottleneck unless [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is used\.

<ins>Pros</ins>: 

- Easy to implement\.
- Dynamic scalability\.
- Failure of a single instance affects few users\.
- [*Canary Release*](https://martinfowler.com/bliki/CanaryRelease.html) is supported\.


<ins>Cons</ins>: 

- The database limits the system’s scalability and performance\.
- The *Load Balancer* and *Shared Database* increase latency and are single points of failure\.


## Dedicate an instance to each client

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Instance%20per%20Client.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Instance%20per%20Client.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Instance%20per%20Client.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Instance%20per%20Client.png" alt="Monolith to Instance per Client" loading="lazy" width="1103" height="343" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Create on Demand]({{< relref "../../basic-metapatterns/shards.md#temporary-state-create-on-demand" >}}) \([Shards]({{< relref "../../basic-metapatterns/shards.md" >}})\), [Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}}), [Virtualizer]({{< relref "../../implementation-metapatterns/microkernel.md#virtualizer-hypervisor-container-orchestrator-distributed-runtime" >}}) \([Microkernel]({{< relref "../../implementation-metapatterns/microkernel.md" >}})\), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: very low latency, dynamic scalability, and failure isolation\.

<ins>Prerequisite</ins>: each client’s data is small and independent of other clients\.

Each client gets an instance of the application which preloads their data into memory\. This way all the data is instantly accessible and a processing fault from one client never affects the other clients\. As systems tend to have thousands to millions of clients, it is inefficient to spawn a process per client\. Instead, more lightweight entities are used: a web app in a browser or an *actor* in a [distributed framework]({{< relref "../../basic-metapatterns/services.md#distributed-runtime-function-as-a-service-faas-including-nanoservices-backend-actors" >}})\.

<ins>Pros</ins>: 

- Nearly perfect dynamic scalability \(limited by the persistence layer\)\.
- Good latency as everything happens in RAM\.
- Fault isolation is one of the features of distributed frameworks\.
- Frameworks are available out of the box\.


<ins>Cons</ins>: 

- Virtualization frameworks tend to introduce a performance penalty\.
- You may need to learn an uncommon technology\.
- Scalability and performance are still limited by the shared persistence layer\.


## Further steps

In most cases *sharding* does not change much inside the application, thus the common evolutions for [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) \(to [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}), [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}), and [*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}})\) remain applicable after sharding\. We’ll focus on their scalability:

- [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) can be scaled \(often to a dramatic extent\) and deployed individually, as [exemplified by the *Three\-Tier Architecture*]({{< relref "../../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#distribution" >}})\.
- [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) allow for subdomains to scale independently with the help of [*Load Balancers*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) or a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}})\. They also improve performance of data storage as each service uses its own database which is often chosen to best fit its distinct needs\.
- Granular scaling can apply to [*Pipelines*]({{< relref "../../basic-metapatterns/pipeline.md" >}}), but in many cases that does not make much sense as pipeline components tend to be lightweight and stateless, making it easy to scale the pipeline as a whole\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%201.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%201.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%201.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%201.png" alt="Monolith to Shards - Further 1" loading="lazy" width="1244" height="363" style="width:100%"/>
</picture>
</a>
</figure>

There are specific evolutions of [*Shards*]({{< relref "../../basic-metapatterns/shards.md" >}}) that deal with their drawbacks:

- [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) reimplements [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) with [*Mesh*]({{< relref "../../implementation-metapatterns/mesh.md" >}})\. Its main goal is to make the data layer dynamically scalable, but the exact results are limited by the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) thus, depending on the mode of action, it can provide very high performance with no consistency guarantees for a small dataset, or reasonable performance for a huge dataset\. It blends the best features of stateful [*Shards*]({{< relref "../../basic-metapatterns/shards.md" >}}) and [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \(being an option for either to evolve to\) but may be quite expensive to run and lacks algorithmic support for analytical queries\.
- [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) is a mirror image of [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}), another option to implement use cases that deal with data of multiple shards without the need for the shards to intercommunicate\. Stateless *Orchestrators* scale perfectly but may corrupt the data if two of them write to an overlapping set of records\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%202.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%202.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Shards%20-%20Further%202.png" alt="Monolith to Shards - Further 2" loading="lazy" width="1243" height="346" style="width:100%"/>
</picture>
</a>
</figure>