+++
weight = 5
title = "The multiform read-write separation"
description = "Patterns that separate read and write paths include: read-only database replicas, response cache, CQRS, Data Mesh, MVCC, and (Re)Actor-with-Extractors."
images = ["/diagrams/Web/og/Read-write.png"]
[sitemap]
  priority = 0.5
+++

# The multiform read\-write separation {anchor=false}

When people discuss read\-write separation at the system level, they tend to imply [*CQRS*](https://martinfowler.com/bliki/CQRS.html) which dedicates a [pair of services]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}), one each for handling read and write requests\. However, there are other ways to subdivide a system, and each of them can be used with read\-write separation:

## [Replicas]({{< relref "../../basic-metapatterns/shards.md#persistent-copy-replica" >}})

We can deploy [*read\-only replicas*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#read-only-replicas" >}}) of the system’s database for nearly unlimited scalability of read traffic without introducing distributed transactions or the need to resolve write conflicts\. This is because all the writes go to the same database replica\. However, the system still suffers from [*replication lag*](https://vibeengines.com/glossary/replication-lag) between the write and read replicas\.

<figure>
<a href="/diagrams/Variants/3/Read-only%20Replica.png">
<picture>
<source srcset="/diagrams/Variants/3/Read-only%20Replica.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/Read-only%20Replica.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/Read-only%20Replica.png" alt="An instance of a backend writes to a leader database which streams updates to database replicas. Other backend instances read from the replicas." loading="lazy" width="713" height="425" style="width:79%"/>
</picture>
</a>
</figure>

## [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})

It is common to cache a subset of the system’s state in one of its upper layers so that the majority of read requests are answered without the need to access the rest of the system’s components\.

A [*Response Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) saves responses to the latest client *queries* \(read requests\) and reuses them to reply to matching incoming requests\. It cannot help with *commands* \(write request\) because a command will change something inside the system, and additionally even the cache itself must be updated after a command passes through it\.

<figure>
<a href="/diagrams/Conclusion/RW-Cache.png">
<picture>
<source srcset="/diagrams/Conclusion/RW-Cache.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/RW-Cache.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/RW-Cache.png" alt="The cache layer remembers responses from the application and reuses them for incoling client queries." loading="lazy" width="1042" height="422" style="width:100%"/>
</picture>
</a>
</figure>

<aside>

> The diagram shows *Invalidate\-On\-Write* caching\. There are other options with specific benefits and drawbacks\.

</aside>

[*Control software*]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) usually involves an integrated *model* for the system it controls\. Its algorithms make decisions based on the embedded model without querying the real hardware\. And once a decision is made, it is then enacted in the hardware\.

<figure>
<a href="/diagrams/Conclusion/RW-Control.png">
<picture>
<source srcset="/diagrams/Conclusion/RW-Control.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/RW-Control.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/RW-Control.png" alt="The control layer repeatedly queries the model layer and finally decides to send a request which is propagated down to the hardware components." loading="lazy" width="823" height="323" style="width:100%"/>
</picture>
</a>
</figure>

## [Services]({{< relref "../../basic-metapatterns/services.md" >}})

One or two of the system’s layers may serve commands and queries with functionally different components\.

[*Command Query Responsibility Segregation*]({{< relref "../../extension-metapatterns/sandwich.md#command-query-responsibility-segregation-cqrs" >}}) \(*CQRS*\) processes the commands and queries in separate modules or services\. That can be beneficial because one of the main concerns with commands is keeping the edited record’s data self\-consistent while queries often focus on aggregating multiple records\. These two activities have very little in common and even differ in the optimal representation of the data they process, therefore they are easy to separate\.

<figure>
<a href="/diagrams/Variants/2/CQRS.png">
<picture>
<source srcset="/diagrams/Variants/2/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/CQRS.png" alt="A large read and smaller write models between a user interface and database." loading="lazy" width="883" height="263" style="width:100%"/>
</picture>
</a>
</figure>

[*Online Transaction Processing*](https://en.wikipedia.org/wiki/Online_transaction_processing) \(*OLTP*\) and [*Online Analytical Processing*](https://en.wikipedia.org/wiki/Online_analytical_processing) \(*OLAP*\), being the names for command\- and query\-optimized databases, respectively, bring read\-write separation to the [data layer]({{< relref "../../basic-metapatterns/layers.md#data-persistence" >}})\. The [pair of OLTP and OLAP databases]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) used by a \(sub\)system may differ in their schema, engine, and even type \(SQL vs NoSQL\)\. Replication lag applies here as well\.

<figure>
<a href="/diagrams/Conclusion/RW-OLTP-OLAP.png">
<picture>
<source srcset="/diagrams/Conclusion/RW-OLTP-OLAP.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/RW-OLTP-OLAP.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/RW-OLTP-OLAP.png" alt="Write requests go to the OLTP database. Read requests go to the OLAP database. The OLTP database streams changes to the OLAP database." loading="lazy" width="843" height="343" style="width:90%"/>
</picture>
</a>
</figure>

CQRS and the OLAP/OLTP separation or read\-only database replicas [often go together]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}):

<figure>
<a href="/diagrams/Conclusion/RW-CQRS-Options.png">
<picture>
<source srcset="/diagrams/Conclusion/RW-CQRS-Options.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/RW-CQRS-Options.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/RW-CQRS-Options.png" alt="The domain-level separation of CQRS can be combined with the data-level separation of read-only database replicas or a pair of OLTP and OLTP databases." loading="lazy" width="863" height="283" style="width:100%"/>
</picture>
</a>
</figure>

## Time division

Surprisingly, some systems rely on read and write separation along the time axis\. In simple terms, it’s like using a system\-wide [readers\-writer lock](https://en.wikipedia.org/wiki/Readers%E2%80%93writer_lock)\.

In [*\(Re\)Actor\-with\-Extractors*]({{< relref "../../basic-metapatterns/monolith.md#inexact-reactor-with-extractors-phased-processing" >}}), the entire system \(e\.g\. a computer game engine\) undergoes alternating read and write phases, allowing for efficient lock\-free simulation of multiple interacting objects\.

<figure>
<a href="/diagrams/Variants/1/Reactor%20with%20Extractors.png">
<picture>
<source srcset="/diagrams/Variants/1/Reactor%20with%20Extractors.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Reactor%20with%20Extractors.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Reactor%20with%20Extractors.png" alt="In the extraction phase components call each other and add actions to their queues. In the reaction phase they execute the actions from their queues but don't interact. The phases alternate." loading="lazy" width="707" height="483" style="width:100%"/>
</picture>
</a>
</figure>

[*Multiversion Concurrency Control*](https://en.wikipedia.org/wiki/Multiversion_concurrency_control) \(*MVCC*\) assigns each database request to a data [*snapshot*](https://en.wikipedia.org/wiki/Snapshot_isolation): queries operate on the current snapshot while a command requires a new snapshot to be allocated for future changes\. That resolves the [architectural forces conflict]({{< relref "../../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#conflicting-forces" >}}) between long\-running queries and real\-time updates at the cost of the queries returning stale data\.

<figure>
<a href="/diagrams/Conclusion/RW-MVCC.png">
<picture>
<source srcset="/diagrams/Conclusion/RW-MVCC.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/RW-MVCC.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/RW-MVCC.png" alt="The database creates a snapshot of its data for every incoming write request. A read request is served from the latest snapshot. Unused snapshots are deleted." loading="lazy" width="1043" height="362" style="width:100%"/>
</picture>
</a>
</figure>

## Interleaving subsystems

A [*Data Mesh*]({{< relref "../../basic-metapatterns/pipeline.md#data-mesh" >}}) builds a graph of analytical datastores, called *Data Product Quanta* \(*DPQ*\), which exists in parallel to and is co\-located with the system it extracts data from\. That almost completely decouples the transactional and analytical data flows\.

<figure>
<a href="/diagrams/Variants/1/Data%20Mesh.png">
<picture>
<source srcset="/diagrams/Variants/1/Data%20Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Data%20Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Data%20Mesh.png" alt="Data Mesh builds an extra graph of services that stream and process analytical data." loading="lazy" width="1094" height="264" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

[*CQRS*]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) is far from being the only way to separate read and write requests at the system level\. Many other architectural patterns, from the ubiquitous [*Response Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) to much less well known [*\(Re\)Actor\-with\-Extractors*]({{< relref "../../basic-metapatterns/monolith.md#inexact-reactor-with-extractors-phased-processing" >}}) and [*Data Mesh*]({{< relref "../../basic-metapatterns/pipeline.md#data-mesh" >}}), apply the same principle in ingenious ways\.