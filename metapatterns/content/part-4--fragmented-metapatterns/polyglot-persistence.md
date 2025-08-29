+++
weight = 7
title = "Polyglot Persistence"
+++

# Polyglot Persistence

<p align="center">
<img src="/Main/Polyglot Persistence.png" alt="Polyglot Persistence" width=100%/>
</p>

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
- [Reporting Database](https://martinfowler.com/bliki/ReportingDatabase.html) / CQRS View Database \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] / Event\-Sourced View \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] / Source\-Aligned \(Native\) Data Product Quantum \(DPQ\) of Data Mesh \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\],
- [Memory Image](https://martinfowler.com/bliki/MemoryImage.html) / Materialized View \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\],
- Query Service \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] / Front Controller \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / Data Warehouse \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\] / Data Lake \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\] / Aggregate Data Product Quantum \(DPQ\) of Data Mesh \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\],
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


<ins>References:</ins> The [original](https://martinfowler.com/bliki/PolyglotPersistence.html) and closely related [CQRS](https://martinfowler.com/bliki/CQRS.html) articles from Martin Fowler, chapter 7 of \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\], chapter 11 of \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] and much information dispersed all over the Web\.

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

<p align="center">
<img src="/Dependencies/PolyglotPersistence.png" alt="PolyglotPersistence" width=100%/>
</p>

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

<p align="center">
<img src="/Relations/Polyglot Persistence.png" alt="Polyglot Persistence" width=100%/>
</p>

*Polyglot Persistence*:

- Extends [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), or [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.
- Is derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) \(persistence layer\) or [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.
- Variants with derived databases have an aspect of [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) and are closely related to [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.


## Variants with independent storage

Many cases of *Polyglot Persistence* use multiple datastores just because there is no single technology that matches all the application’s needs\. The databases used are filled with different subsets of the system’s data:

### Specialized Databases

<p align="center">
<img src="/Variants/3/PP - Specialized.png" alt="PP - Specialized" width=100%/>
</p>

Databases [vary in their optimal use cases](https://www.jamesserra.com/archive/2015/07/what-is-polyglot-persistence/)\. You can employ several different databases to achieve the best performance for each kind of data that you persist\.

### Private and Shared Databases

<p align="center">
<img src="/Variants/3/PP - Private and Shared.png" alt="PP - Private and Shared" width=100%/>
</p>

If several services or shards become coupled through a subset of the system’s data, that subset can be put into a separate database which is accessible to all the participants\. All the other data remains private to the [shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) or [services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

### Data File, Content Delivery Network \(CDN\)

<p align="center">
<img src="/Variants/3/PP - File Storage.png" alt="PP - File Storage" width=100%/>
</p>

Some data is happy to stay in files\. Web frameworks load web page templates from OS files and store images and videos in a *Content Delivery Network* \(*CDN*\) which replicates the data all over the world so that each user downloads the content from the nearest server \(which is faster and cheaper\)\.

## Variants with derived storage

In other cases there is a single writable database \(*system of record* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\) which is the main *source of truth* from which the other databases are derived\. The primary reason to use several databases is to [relieve the main database of read requests]({{< relref "#read-only-replica" >}}) and maybe support some additional qualities: special kinds of queries, aggregation for [*materialized*]({{< relref "#memory-image-materialized-view" >}}) and [*CQRS views*]({{< relref "#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}), full text search for [*text indices*]({{< relref "#external-search-index" >}}), huge dataset size for [*historical data*]({{< relref "#historical-data-data-archiving" >}}) or low latency for an [*in\-memory cache*]({{< relref "#database-cache-cache-aside" >}})\.

The updates to the derived databases may come from:

- the main database as [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\) \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] \(a log of changes\),
- the application after it changes the main database \(see caching strategies below\),
- another service as *event stream* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}}), [MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\],
- a dedicated *indexer* that periodically crawls the main database or web site\.


<p align="center">
<img src="/Variants/3/PP - Derived Storage.png" alt="PP - Derived Storage" width=100%/>
</p>

### Read\-Only Replica

<p align="center">
<img src="/Variants/3/Read-only Replica.png" alt="Read-only Replica" width=71%/>
</p>

Multiple instances of the database are deployed and one of them is the *leader* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] instance which processes all writes to the system’s data\. The changes are then replicated to the other instances \(via [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\)\) which are used for read requests\. Distributing workload over multiple instances increases maximum read throughput which the system is capable of, as the database is usually the system’s bottleneck\. Having several running [*replicas*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) greatly improves reliability and allows for nearly instant recovery of database failures as any replica may quickly be promoted to the leader role to serve write traffic\.

### Reporting Database, CQRS View Database, Event\-Sourced View, Source\-Aligned \(Native\) Data Product Quantum \(DPQ\) of Data Mesh

<p align="center">
<img src="/Variants/3/Reporting DB and CQRS View.png" alt="Reporting DB and CQRS View" width=100%/>
</p>

It is common wisdom that a database is good for either *OLTP* \(transactions\) or *OLAP* \(queries\)\. Here we have two databases: one optimized for commands \(write traffic protected with transactions\) and another one for complex analytical queries\. The databases differ at least in schema \(OLAP schema is optimized for queries\) and often vary in type \(e\.g\. SQL vs NoSQL\)\.

A [*Reporting Database*](https://martinfowler.com/bliki/ReportingDatabase.html) \(or *Source\-Aligned \(Native\) Data Product Quantum* of [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\]\) derives its data from a write\-enabled database in the same subsystem \(service\) while a *CQRS View* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] or *Event\-Sourced View* \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] is fed a stream of events from another service from which it filters the data relevant to its owner\. This way a *CQRS View* lets its owner service query \(its replica of\) the data that originally belonged to other services\.

### Memory Image, Materialized View

<p align="center">
<img src="/Variants/3/Memory Image.png" alt="Memory Image" width=100%/>
</p>

*Event sourcing* \(of [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) or [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}})\) is all about changes\. A service persists only *changes* to its data instead of the *current* data\. As a result, the service needs to aggregate its history into a [*Memory Image*](https://martinfowler.com/bliki/MemoryImage.html) \(*Materialized View* \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\) by loading a snapshot and replaying any further events to rebuild its current state \(which other architectural styles store in databases\) and start operating\.

### Query Service, Front Controller, Data Warehouse, Data Lake, Aggregate Data Product Quantum \(DPQ\) of Data Mesh

<p align="center">
<img src="/Variants/3/Query Service.png" alt="Query Service" width=100%/>
</p>

A *Query Service* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] \(or *Aggregate Data Product Quantum* of [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\]\) subscribes to events from several full\-featured services and aggregates them into its database, making it a [*CQRS View*]({{< relref "#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) of several services or even the whole system\. If any other service or a data analyst needs to process data which belongs to multiple services, it retrieves it from the *Query Service* which has already joined the data streams and represents the join in a convenient way\.

A [*Front Controller*]({{< relref "../part-3--extension-metapatterns/combined-component.md#front-controller" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] is a *Query Service* embedded in the first \(user\-facing\) service of a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\. It collects status updates from downstream components of the *Pipeline* to track the state of every request being processed by the *Pipeline*\.

*Data Warehouse* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\] and *Data Lake* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\] are *analytical* databases that connect directly to and import all the data from the *operational* \(main\) databases of all the system’s services\. A *Data Warehouse* translates the imported data into its own unified schema while a *Data Lake* stores the imported data in its original formats\.

### External Search Index

<p align="center">
<img src="/Variants/3/Search Index.png" alt="Search Index" width=100%/>
</p>

Some domains require a kind of search which is not naturally supported by ordinary database engines\. Full text search, especially [NLP](https://en.wikipedia.org/wiki/Natural_language_processing)\-enabled, is one such case\. Geospatial data may be another\. If you are comfortable with your main database\(s\), you can set up an *External Search Index* by deploying a product dedicated to the special kind of search that you need and feeding it updates from your main database\.

### Historical Data, Data Archiving

<p align="center">
<img src="/Variants/3/Historical Data.png" alt="Historical Data" width=92%/>
</p>

It is common to store the history of sales in a database\. However, once a month or two has passed, it is very unlikely that the historical records will ever be edited\. And though they are queried on very rare occasions, like audits, they still slow down your database\. Some businesses offload any data older than a couple of months to a cheaper [*archive storage*](https://www.datacore.com/glossary/what-is-data-archiving/) which does not allow changes to the data and has limited query capabilities in order to keep the main datasets small and fast\.

### Database Cache, Cache\-Aside

<p align="center">
<img src="/Variants/3/Cache-Aside.png" alt="Cache-Aside" width=80%/>
</p>

Database queries are resource\-heavy while databases scale only to a limited extent\. That means that a highly loaded system benefits from bypassing its main database with as many queries as possible, that is usually achieved by storing recent queries and their results in an in\-memory database \([*Cache\-Aside*](https://www.enjoyalgorithms.com/blog/cache-aside-caching-strategy)\)\. Each incoming query is first looked for in the fast cache, and if it is found then you are lucky to get the result immediately without having to consult the main database\.

Keeping the cache consistent with the main database is the hard part\. There are quite a few strategies \(some of them treat the [cache as a *Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) for the database\): [write\-through](https://www.enjoyalgorithms.com/blog/write-through-caching-strategy), [write\-behind](https://www.enjoyalgorithms.com/blog/write-behind-caching-pattern), [write\-around](https://www.enjoyalgorithms.com/blog/write-around-caching-pattern) and [refresh\-ahead](https://www.enjoyalgorithms.com/blog/refresh-ahead-caching-pattern)\.

## Evolutions

*Polyglot Persistence* with derived storage can often be made subject to *CQRS*:

- The service that uses the read and write databases is [split into separate read and write services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\.


<p align="center">
<img src="/Evolutions/3/Polyglor Persistence - 1.png" alt="Polyglor Persistence - 1" width=100%/>
</p>

## Summary

*Polyglot Persistence* employs several specialized databases to improve performance, often at the cost of eventual data consistency or implementing transactions in the application\.

| \<\< [Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Backends for Frontends \(BFF\)]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \>\> |
| --- | --- | --- |


