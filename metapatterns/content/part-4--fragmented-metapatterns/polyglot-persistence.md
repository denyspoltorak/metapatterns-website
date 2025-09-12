+++
weight = 7
title = "Polyglot Persistence"
description = Polyglot Persistence is the pattern for using multiple databases, which may be mutually independent or derived. The specialization improves performance.
+++

# Polyglot Persistence

<figure style="text-align:center">
<a href="/Main/Polyglot%20Persistence.png" style="outline:none">
<img src="/Main/Polyglot%20Persistence.png" alt="Polyglot Persistence" style="width:100%"/>
</a>
</figure>

*Unbind your data\.* Use multiple specialized databases\.

<ins>Known as:</ins> Polyglot Persistence\.

<ins>Aspects:</ins> those of the [databases]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) involved\.

<ins>Variants:</ins> 

Independent storage:

- Specialized Databases,
- Private and Shared Databases,
- Data File / Content Delivery Network \(CDN\)\.


Derived storage:

- Read\-Only Replica,
- [Reporting Database](https://martinfowler.com/bliki/ReportingDatabase.html) / CQRS View Database \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / Event\-Sourced View \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\] / Source\-Aligned \(Native\) Data Product Quantum \(DPQ\) of Data Mesh \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\],
- [Memory Image](https://martinfowler.com/bliki/MemoryImage.html) / Materialized View \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\],
- Query Service \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / Front Controller \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] / Data Warehouse \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] / Data Lake \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] / Aggregate Data Product Quantum \(DPQ\) of Data Mesh \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\],
- External Search Index,
- Historical Data / [Data Archiving](https://www.datacore.com/glossary/what-is-data-archiving/),
- Database Cache / [Cache\-Aside](https://www.enjoyalgorithms.com/blog/cache-aside-caching-strategy)\.


<ins>Structure:</ins> A layer of data services used by higher\-level components\.

<ins>Type:</ins> Extension, derived from [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Performance is fine\-tuned for various data types and use cases | The peculiarities of each database need to be learned |
| Less load on each database | Muсh more work for the DevOps team |
| The databases may satisfy conflicting forces | More points of failure in the system |
|  | Consistency is hard or slow to achieve |


<ins>References:</ins> The [original](https://martinfowler.com/bliki/PolyglotPersistence.html) and closely related [CQRS](https://martinfowler.com/bliki/CQRS.html) articles from Martin Fowler, chapter 7 of \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\], chapter 11 of \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] and much information dispersed all over the Web\.

You can choose a dedicated technology for each kind of data or pattern of data access in your system\. That improves performance \(as each database engine is optimized for a few use cases\), distributes load between the databases, and may solve conflicts between forces \(like when you need both low latency and large storage\)\. However, you’ll likely have to hire several experts to get the best use of and to support the multiple databases\. Moreover, having your data spread over multiple databases makes it the application’s responsibility to keep the data in sync \(by implementing some kind of distributed transactions or making sure that the clients don’t get stale data\)\.

### Performance

*Polyglot Persistence* aims at improving performance through the following means:

- Optimize for [specific data use cases]({{< relref "#specialized-databases" >}})\. It is impossible for a single database to be good at everything\.
- Redirect read traffic to [read\-only database *replicas*]({{< relref "#read-only-replica" >}})\. The write\-enabled *leader* database then processes only the write requests\.
- [*Cache* any frequently used data]({{< relref "#database-cache-cache-aside" >}}) in a fast in\-memory database to let the majority of client requests be served without hitting the slower persistent storage\.
- Build a [*view* of the states of other services]({{< relref "#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) in the system to avoid querying them\.
- Maintain an external [*index*]({{< relref "#external-search-index" >}}) or [*Memory Image*]({{< relref "#memory-image-materialized-view" >}}) for use with tasks that don’t need the history of changes\.
- [Purge old data]({{< relref "#historical-data-data-archiving" >}}) to a slower storage\.
- Store read\-only sequential [data as files]({{< relref "#data-file-content-delivery-network-cdn" >}}), often close to the end users who download them\.


Read\-write separation introduces a [replication lag](https://medium.com/@Ian_carson/replication-lag-82c736081e32) which is a pain when data consistency is important for the system’s clients\.

### Dependencies

In general, each service depends on all of the databases which it uses\. There may also be an additional dependency between the databases if they share a dataset \(one or more databases are derived\)\.

<figure style="text-align:center">
<a href="/Dependencies/PolyglotPersistence.png" style="outline:none">
<img src="/Dependencies/PolyglotPersistence.png" alt="PolyglotPersistence" style="width:100%"/>
</a>
</figure>

### Applicability

*Polyglot Persistence* <ins>helps</ins>:

- *High load and low latency projects\.* [*Specialized Databases*]({{< relref "#specialized-databases" >}}) shine when given fitting tasks\. [*Caching*]({{< relref "#database-cache-cache-aside" >}}) and [*Read\-Only Replicas*]({{< relref "#read-only-replica" >}}) take the load off the main database\. [*External Search Indices*]({{< relref "#external-search-index" >}}) save the day\.
- *Event sourcing\.* [*Materialized Views*]({{< relref "#memory-image-materialized-view" >}}) maintain the current states of the system’s components\.
- *Conflicting forces*\. An instance of a stateless service inherits many of the qualities of the database which it accesses for any given request it is processing\. When there are several databases, the qualities of a service instance may vary from request to request,depending on which database is involved\.


*Polyglot Persistence* may <ins>harm</ins>:

- *Small projects\.* Properly setting up and maintaining multiple databases is not that easy\.
- *High availability*\. Each database which your system uses will tend to fail in its own crazy way\.
- *User experience*\. For systems with read\-write database separation the replication lag between the databases will make you [choose](https://medium.com/@Ian_carson/replication-lag-82c736081e32) between reading changes from the *leader* \(write database\), adding synchronization code to your application to wait for the read database to be updated, and risking returning outdated results to the users\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Polyglot%20Persistence.png" style="outline:none">
<img src="/Relations/Polyglot%20Persistence.png" alt="Polyglot Persistence" style="width:100%"/>
</a>
</figure>

*Polyglot Persistence*:

- Extends [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), or [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.
- Is derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) \(persistence layer\) or [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.
- Variants with derived databases have an aspect of [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) and are closely related to [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.


## Variants with independent storage

Many cases of *Polyglot Persistence* use multiple datastores just because there is no single technology that matches all the application’s needs\. The databases used are filled with different subsets of the system’s data:

### Specialized Databases

<figure style="text-align:center">
<a href="/Variants/3/PP%20-%20Specialized.png" style="outline:none">
<img src="/Variants/3/PP%20-%20Specialized.png" alt="PP - Specialized" style="width:100%"/>
</a>
</figure>

Databases [vary in their optimal use cases](https://www.jamesserra.com/archive/2015/07/what-is-polyglot-persistence/)\. You can employ several different databases to achieve the best performance for each kind of data that you persist\.

### Private and Shared Databases

<figure style="text-align:center">
<a href="/Variants/3/PP%20-%20Private%20and%20Shared.png" style="outline:none">
<img src="/Variants/3/PP%20-%20Private%20and%20Shared.png" alt="PP - Private and Shared" style="width:100%"/>
</a>
</figure>

If several services or shards become coupled through a subset of the system’s data, that subset can be put into a separate database which is accessible to all the participants\. All the other data remains private to the [shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) or [services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

### Data File, Content Delivery Network \(CDN\)

<figure style="text-align:center">
<a href="/Variants/3/PP%20-%20File%20Storage.png" style="outline:none">
<img src="/Variants/3/PP%20-%20File%20Storage.png" alt="PP - File Storage" style="width:100%"/>
</a>
</figure>

Some data is happy to stay in files\. Web frameworks load web page templates from OS files and store images and videos in a *Content Delivery Network* \(*CDN*\) which replicates the data all over the world so that each user downloads the content from the nearest server \(which is faster and cheaper\)\.

## Variants with derived storage

In other cases there is a single writable database \(*system of record* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\]\) which is the main *source of truth* from which the other databases are derived\. The primary reason to use several databases is to [relieve the main database of read requests]({{< relref "#read-only-replica" >}}) and maybe support some additional qualities: special kinds of queries, aggregation for [*materialized*]({{< relref "#memory-image-materialized-view" >}}) and [*CQRS views*]({{< relref "#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}), full text search for [*text indices*]({{< relref "#external-search-index" >}}), huge dataset size for [*historical data*]({{< relref "#historical-data-data-archiving" >}}) or low latency for an [*in\-memory cache*]({{< relref "#database-cache-cache-aside" >}})\.

The updates to the derived databases may come from:

- the main database as [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\) \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] \(a log of changes\),
- the application after it changes the main database \(see caching strategies below\),
- another service as *event stream* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}}), [MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\],
- a dedicated *indexer* that periodically crawls the main database or web site\.


<figure style="text-align:center">
<a href="/Variants/3/PP%20-%20Derived%20Storage.png" style="outline:none">
<img src="/Variants/3/PP%20-%20Derived%20Storage.png" alt="PP - Derived Storage" style="width:100%"/>
</a>
</figure>

### Read\-Only Replica

<figure style="text-align:center">
<a href="/Variants/3/Read-only%20Replica.png" style="outline:none">
<img src="/Variants/3/Read-only%20Replica.png" alt="Read-only Replica" style="width:71%"/>
</a>
</figure>

Multiple instances of the database are deployed and one of them is the *leader* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] instance which processes all writes to the system’s data\. The changes are then replicated to the other instances \(via [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\)\) which are used for read requests\. Distributing workload over multiple instances increases maximum read throughput which the system is capable of, as the database is usually the system’s bottleneck\. Having several running [*replicas*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) greatly improves reliability and allows for nearly instant recovery of database failures as any replica may quickly be promoted to the leader role to serve write traffic\.

### Reporting Database, CQRS View Database, Event\-Sourced View, Source\-Aligned \(Native\) Data Product Quantum \(DPQ\) of Data Mesh

<figure style="text-align:center">
<a href="/Variants/3/Reporting%20DB%20and%20CQRS%20View.png" style="outline:none">
<img src="/Variants/3/Reporting%20DB%20and%20CQRS%20View.png" alt="Reporting DB and CQRS View" style="width:100%"/>
</a>
</figure>

It is common wisdom that a database is good for either *OLTP* \(transactions\) or *OLAP* \(queries\)\. Here we have two databases: one optimized for commands \(write traffic protected with transactions\) and another one for complex analytical queries\. The databases differ at least in schema \(OLAP schema is optimized for queries\) and often vary in type \(e\.g\. SQL vs NoSQL\)\.

A [*Reporting Database*](https://martinfowler.com/bliki/ReportingDatabase.html) \(or *Source\-Aligned \(Native\) Data Product Quantum* of [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\]\) derives its data from a write\-enabled database in the same subsystem \(service\) while a *CQRS View* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] or *Event\-Sourced View* \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\] is fed a stream of events from another service from which it filters the data relevant to its owner\. This way a *CQRS View* lets its owner service query \(its replica of\) the data that originally belonged to other services\.

### Memory Image, Materialized View

<figure style="text-align:center">
<a href="/Variants/3/Memory%20Image.png" style="outline:none">
<img src="/Variants/3/Memory%20Image.png" alt="Memory Image" style="width:100%"/>
</a>
</figure>

*Event sourcing* \(of [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) or [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}})\) is all about changes\. A service persists only *changes* to its data instead of the *current* data\. As a result, the service needs to aggregate its history into a [*Memory Image*](https://martinfowler.com/bliki/MemoryImage.html) \(*Materialized View* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\]\) by loading a snapshot and replaying any further events to rebuild its current state \(which other architectural styles store in databases\) and start operating\.

### Query Service, Front Controller, Data Warehouse, Data Lake, Aggregate Data Product Quantum \(DPQ\) of Data Mesh

<figure style="text-align:center">
<a href="/Variants/3/Query%20Service.png" style="outline:none">
<img src="/Variants/3/Query%20Service.png" alt="Query Service" style="width:100%"/>
</a>
</figure>

A *Query Service* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] \(or *Aggregate Data Product Quantum* of [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\]\) subscribes to events from several full\-featured services and aggregates them into its database, making it a [*CQRS View*]({{< relref "#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) of several services or even the whole system\. If any other service or a data analyst needs to process data which belongs to multiple services, it retrieves it from the *Query Service* which has already joined the data streams and represents the join in a convenient way\.

A [*Front Controller*]({{< relref "../part-3--extension-metapatterns/combined-component.md#front-controller" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] is a *Query Service* embedded in the first \(user\-facing\) service of a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\. It collects status updates from downstream components of the *Pipeline* to track the state of every request being processed by the *Pipeline*\.

*Data Warehouse* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] and *Data Lake* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] are *analytical* databases that connect directly to and import all the data from the *operational* \(main\) databases of all the system’s services\. A *Data Warehouse* translates the imported data into its own unified schema while a *Data Lake* stores the imported data in its original formats\.

### External Search Index

<figure style="text-align:center">
<a href="/Variants/3/Search%20Index.png" style="outline:none">
<img src="/Variants/3/Search%20Index.png" alt="Search Index" style="width:100%"/>
</a>
</figure>

Some domains require a kind of search which is not naturally supported by ordinary database engines\. Full text search, especially [NLP](https://en.wikipedia.org/wiki/Natural_language_processing)\-enabled, is one such case\. Geospatial data may be another\. If you are comfortable with your main database\(s\), you can set up an *External Search Index* by deploying a product dedicated to the special kind of search that you need and feeding it updates from your main database\.

### Historical Data, Data Archiving

<figure style="text-align:center">
<a href="/Variants/3/Historical%20Data.png" style="outline:none">
<img src="/Variants/3/Historical%20Data.png" alt="Historical Data" style="width:92%"/>
</a>
</figure>

It is common to store the history of sales in a database\. However, once a month or two has passed, it is very unlikely that the historical records will ever be edited\. And though they are queried on very rare occasions, like audits, they still slow down your database\. Some businesses offload any data older than a couple of months to a cheaper [*archive storage*](https://www.datacore.com/glossary/what-is-data-archiving/) which does not allow changes to the data and has limited query capabilities in order to keep the main datasets small and fast\.

### Database Cache, Cache\-Aside

<figure style="text-align:center">
<a href="/Variants/3/Cache-Aside.png" style="outline:none">
<img src="/Variants/3/Cache-Aside.png" alt="Cache-Aside" style="width:80%"/>
</a>
</figure>

Database queries are resource\-heavy while databases scale only to a limited extent\. That means that a highly loaded system benefits from bypassing its main database with as many queries as possible, that is usually achieved by storing recent queries and their results in an in\-memory database \([*Cache\-Aside*](https://www.enjoyalgorithms.com/blog/cache-aside-caching-strategy)\)\. Each incoming query is first looked for in the fast cache, and if it is found then you are lucky to get the result immediately without having to consult the main database\.

Keeping the cache consistent with the main database is the hard part\. There are quite a few strategies \(some of them treat the [cache as a *Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) for the database\): [write\-through](https://www.enjoyalgorithms.com/blog/write-through-caching-strategy), [write\-behind](https://www.enjoyalgorithms.com/blog/write-behind-caching-pattern), [write\-around](https://www.enjoyalgorithms.com/blog/write-around-caching-pattern) and [refresh\-ahead](https://www.enjoyalgorithms.com/blog/refresh-ahead-caching-pattern)\.

## Evolutions

*Polyglot Persistence* with derived storage can often be made subject to *CQRS*:

- The service that uses the read and write databases is [split into separate read and write services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/3/Polyglor%20Persistence%20-%201.png" style="outline:none">
<img src="/Evolutions/3/Polyglor%20Persistence%20-%201.png" alt="Polyglor Persistence - 1" style="width:100%"/>
</a>
</figure>

## Summary

*Polyglot Persistence* employs several specialized databases to improve performance, often at the cost of eventual data consistency or implementing transactions in the application\.

<nav>

| \<\< [Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Backends for Frontends \(BFF\)]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \>\> |
| --- | --- | --- |

</nav>



