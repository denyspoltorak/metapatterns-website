+++
weight = 9
title = "Layers: improve performance"
+++

# Layers: improve performance

There are several ways to improve the performance of a [*layered system*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}})\. One we have [already discussed]({{< relref "../../part-7--appendices/appendix-e--evolutions/shards--share-data.md#use-space-based-architecture" >}}) for [*Shards*]({{< relref "../../part-2--basic-metapatterns/shards.md" >}}):

- [*Space\-Based Architecture*]({{< relref "../../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) co\-locates the database and business logic and scales both dynamically\.


<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.png" style="outline:none">
<img src="/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.png" alt="Layers to Space-Based Architecture" width=100%/>
</a>
</figure>

Others are new here and thus deserve more attention:

- Merging several layers improves latency by eliminating the communication overhead\.
- Scaling some of the layers may improve throughput but degrade latency\.
- [*Polyglot Persistence*]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) is the name for using multiple specialized databases\.


## Merge several layers

<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers%20Merge.png" style="outline:none">
<img src="/Evolutions/Layers/Layers%20Merge.png" alt="Layers Merge" width=100%/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}) or [Monolith]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}})

<ins>Goal</ins>: improve performance\.

<ins>Prerequisite</ins>: the layers share programming language, hardware setup and qualities\.

If your system’s development [is finished]({{< relref "../../part-6--analytics/architecture-and-product-life-cycle.md#death-the-ultimate-release--monolith" >}}) \(no changes are expected\) and you really need that extra 5% performance improvement, then you can try merging everything back into a *Monolith* or a [*3\-Tier*]({{< relref "../../part-2--basic-metapatterns/layers.md#three-tier-architecture" >}}) system \(front, back, data\)\.

<ins>Pros</ins>: 

- Enables aggressive performance optimizations\.
- The system may become easier to debug\.


<ins>Cons</ins>: 

- The code is frozen – it will be much harder to evolve\.
- Your teams lose the ability to work independently\.


<ins>Further steps</ins>:

- [*Shard*]({{< relref "../../part-2--basic-metapatterns/shards.md" >}}) the entire system\.


## Scale individual layers

<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers_%20Shard.png" style="outline:none">
<img src="/Evolutions/Layers/Layers_%20Shard.png" alt="Layers: Shard" width=100%/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}), [Shards]({{< relref "../../part-2--basic-metapatterns/shards.md" >}}), often [Load Balancer]({{< relref "../../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}})\)\.

<ins>Goal</ins>: scale the system\.

<ins>Prerequisite</ins>: some layers are [stateless]({{< relref "../../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) or limited to the [data of a single client]({{< relref "../../part-2--basic-metapatterns/shards.md#temporary-state-create-on-demand" >}})\.

Multiple instances or layers can be created, with their number and deployment [varying from layer to layer]({{< relref "../../part-1--foundations/forces--asynchronicity--and-distribution.md#distribution" >}})\. That may work seamlessly if each instance of the layer which receives an event which can start a use case knows the instance of the next layer to communicate to\. Otherwise you will need a *Load Balancer*\.

<ins>Pros</ins>: 

- Flexible scalability\.
- Better fault tolerance\.
- Co\-deployment with clients is possible\.


<ins>Cons</ins>: 

- More complex operations \(more parts to keep an eye on\)\.


<ins>Further steps</ins>:

- [*Space\-Based Architecture*]({{< relref "../../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) scales the data layer\.
- [*Polyglot Persistence*]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) improves performance of the data layer\.


## Use multiple databases

<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.png" style="outline:none">
<img src="/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.png" alt="Layers to Polyglot Persistence" width=100%/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}), [Polyglot Persistence]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.

<ins>Goal</ins>: optimize performance of data processing\.

<ins>Prerequisite</ins>: there are isolated use cases for or subsets of the data\.

If you have separated *commands* \(write requests\) from *queries* \(read requests\), you can serve the queries with [read\-only replicas]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of the database while the main database is reserved for the commands\.

If your types of data or data processing algorithms vary, you may deploy several [specialized databases]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md#specialized-databases" >}}), each matching a subset of your needs\. That lets you achieve the best performance in widely diverging cases\.

<ins>Pros</ins>: 

- The best performance for all the use cases\.
- Specialized data processing algorithms out of the box\.
- Replication may help with error recovery\.


<ins>Cons</ins>: 

- Someone will need to learn and administer all those databases\.
- Keeping the databases consistent takes effort and the replication delay may negatively affect UX\.


<ins>Further steps</ins>:

- Serve read and write requests with different backends according to [*Command\-Query Responsibility Segregation \(CQRS\)*]({{< relref "../../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.
- Separate the backend into [*services*]({{< relref "../../part-2--basic-metapatterns/services.md" >}}) which match the already separated databases\.


<nav>

| \<\< [Layers: help large projects]({{< relref "../../part-7--appendices/appendix-e--evolutions/layers--help-large-projects.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Layers: gain flexibility]({{< relref "../../part-7--appendices/appendix-e--evolutions/layers--gain-flexibility.md" >}}) \>\> |
| --- | --- | --- |

</nav>



