+++
weight = 15
title = "Evolutions of a Shared Repository"
description = "A shared database can be sharded or divided into private databases. Space-Based Architecture or Polyglot Persistence help to improve performance."
[sitemap]
  priority = 0.3
+++

# Evolutions of a Shared Repository

Once a database appears, it is unlikely to go away\. I see the following evolutions to improve performance of the data layer:

- [*Shard*]({{< relref "../../basic-metapatterns/shards.md" >}}) the database\.
- Use [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) for dynamic scalability\.
- Divide the data into a private database per service\.
- Deploy specialized databases \([*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


## Shard the database

<figure>
<a href="/diagrams/Evolutions/2/Shared%20Database_%20Shard.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Shared%20Database_%20Shard.png" alt="Shared Database: Shard" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Sharding]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \([Shards]({{< relref "../../basic-metapatterns/shards.md" >}})\), [Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}}), maybe [Sharding Proxy]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\.

<ins>Goal</ins>: increase capacity and performance of the database\.

<ins>Prerequisite</ins>: the data is shardable \(consists of independent records\)\.

If your database is overloaded and the data which it contains describes independent entities \(users, companies, sales\) you can deploy multiple instances of the database with subsets of the data distributed among them\. You will need to deploy a *Sharding Proxy* or the services will have to find out which database *shard* to access by themselves, likely through hashing the record’s *primary key* \[[DDIA]({{< relref "../../appendices/books-referenced.md#ddia" >}})\]\. There is also a good chance that several smaller tables will have to be replicated to all the shards or moved to a [dedicated *Shared Database*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#private-and-shared-databases" >}}) \(resulting in [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}})\)\.

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

- [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) or [*CQRS*]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) describe pre\-calculating aggregates into another analytical database \([*Reporting Database*](https://martinfowler.com/bliki/ReportingDatabase.html)\)\.
- [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) may be cheaper as it scales dynamically\. However, in its default and highly performant configuration it is prone to write collisions\.


## Use Space\-Based Architecture

<figure>
<a href="/diagrams/Evolutions/2/Shared%20Database%20to%20Space-Based%20Architecture.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Shared%20Database%20to%20Space-Based%20Architecture.png" alt="Shared Database to Space-Based Architecture" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Space\-Based Architecture]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) \([Mesh]({{< relref "../../implementation-metapatterns/mesh.md" >}}), [Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: scale throughput of the database dynamically\.

<ins>Prerequisite</ins>: data collisions are acceptable\.

*Space\-Based Architecture* \(SBA\) duplicates contents of a persistent database to a distributed in\-memory *cache* co\-located with the services managed by the [*SBA*’s *Middleware*]({{< relref "../../extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}})\. That makes most database access operations very fast unless one needs to avoid write collisions\. The *Mesh Middleware* autoscales both the services and the associated data cache under load, granting nearly perfect scalability\. However, this architecture is costly because of the amount of traffic and CPU time spent on replicating data between the *Mesh nodes*\.

<ins>Pros</ins>: 

- Nearly unlimited dynamic scalability\.
- Off\-the\-shelf solutions are available\.
- Very high fault tolerance\.


<ins>Cons</ins>: 

- Choose one: data collisions or poor performance\.
- Low latency is guaranteed only when the entire dataset fits in the memory of a node\.
- High operational cost because the nodes will send each other lots of data\.
- No support for analytical queries\.


## Move the data to private databases of services

<figure>
<a href="/diagrams/Evolutions/2/Shared%20Database%20to%20Services.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Shared%20Database%20to%20Services.png" alt="Shared Database to Services" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Services]({{< relref "../../basic-metapatterns/services.md" >}}) or [Shards]({{< relref "../../basic-metapatterns/shards.md" >}}), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: decouple the services or shards, remove the performance bottleneck \(*Shared Database*\)\.

<ins>Prerequisite</ins>: the domain data is weakly coupled\.

If the data clearly follows subdomains, it may be possible to subdivide it accordingly\. The services will become [*choreographed*]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) \(or [*orchestrated*]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) if they have an [*integration layer*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\) instead of communicating through the [shared data]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\.

<ins>Pros</ins>: 

- The services become independent in their persistence and data processing technologies\.
- Performance of the *data layer*, which tends to limit the scalability of the system, will likely improve thanks to the use of smaller specialized databases\.


<ins>Cons</ins>: 

- The communication between the services and the synchronization of their data becomes a major issue\.
- Joins of the data from different subdomains will not be available\.
- Costs are likely to increase because of data transfer and duplication between the services\.
- You will have to administrate multiple databases\.


<ins>Further steps</ins>:

- [*CQRS Views*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] or a [*Query Service*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] help a service access and join data that belongs to other services\.


## Deploy specialized databases

<figure>
<a href="/diagrams/Evolutions/2/Shared%20Database%20to%20Polyglot%20Persistence.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Shared%20Database%20to%20Polyglot%20Persistence.png" alt="Shared Database to Polyglot Persistence" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Polyglot Persistence]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}})\.

<ins>Goal</ins>: improve performance and maybe fault tolerance of the data layer\.

<ins>Prerequisite</ins>: there are diverse data types or patterns of data access\.

It is very likely that you can either use [*specialized databases*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#specialized-databases" >}}) for various data types or deploy [*read\-only replicas*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of your data for analytics\.

<ins>Pros</ins>:

- You can choose one of the many specialized databases available on the market\.
- There is a good chance to significantly improve performance\.
- Replication improves fault tolerance of your data layer\.


<ins>Cons</ins>:

- It may take effort to learn the new technologies and use them efficiently\.
- Someone needs to see to the new database\(s\)\.
- You’ll likely need to work around *replication lag* \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\]\.


<nav>

| \<\< [Evolutions of a Middleware]({{< relref "../../appendices/evolutions/evolutions-of-a-middleware.md" >}}) | ^ [Evolutions]({{< relref "../../appendices/evolutions/_index.md" >}}) ^ | [Evolutions of a Proxy]({{< relref "../../appendices/evolutions/evolutions-of-a-proxy.md" >}}) \>\> |
| --- | --- | --- |

</nav>