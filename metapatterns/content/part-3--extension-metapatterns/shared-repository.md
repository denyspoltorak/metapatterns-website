+++
weight = 7
title = "Shared Repository"
+++

# Shared Repository

<figure style="text-align:center">
<a href="/Main/Shared%20Repository.png" style="outline:none">
<img src="/Main/Shared%20Repository.png" alt="Shared Repository" width=100%/>
</a>
</figure>

*Knowledge itself is power\.* Sharing data is simple \(& stupid\)\.

<ins>Known as:</ins> Shared Repository \[[POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\]\.

<ins>Aspects:</ins>

- Data storage,
- Data consistency,
- Data change notifications\.


<ins>Variants:</ins> 

- Shared Database \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] / [Integration Database](https://martinfowler.com/bliki/IntegrationDatabase.html) / Data Domain \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] / Database of Service\-Based Architecture \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], 
- [Blackboard](https://hillside.net/plop/plop97/Proceedings/lalanda.pdf) \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}}), [POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\],
- Data Grid of [Space\-Based Architecture](https://en.wikipedia.org/wiki/Space-based_architecture) \[[SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sap" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] / Replicated Cache \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] / Distributed Cache, 
- Shared Memory,
- Shared File System,
- \(with a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\) Persistent Event Log / Shared Event Store,
- \(inexact\) Stamp Coupling \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\]\.


<ins>Structure:</ins> A layer of data shared among higher\-level components\.

<ins>Type:</ins> Extension for [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Supports domains with coupled data | A single point of failure |
| Implements data access and synchronization \(consistency\) concerns | All the services depend on the schema of the shared data |
| Helps saving on hardware, licenses, traffic, and administration | A single database technology may not fit the needs of all the services equally well |
| Quick start for a project | Limits scalability |


<ins>References:</ins> \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] is all about databases; \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] has chapters on *Service\-Based Architecture* and *Space\-Based Architecture*; \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\] deals with *Shared Event Store\.*

A *Shared Repository* builds communication in the system around its data, which is natural for [data\-centric domains]({{< relref "../part-1--foundations/arranging-communication/shared-data.md" >}}) and multiple [instances of a stateless service]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) and may often simplify development of a system of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) that need to exchange data\. It covers the following concerns:

- Storage of the entire domain data\.
- Keeping the data self\-consistent by providing atomic transactions for use by the application code\.
- Communication between the services \(if the repository supports notifications on data change\)\.


The drawbacks are extensive coupling \(it’s hard to alter a thing which is used in many places throughout the entire system\) and limited scalability \(even distributed databases struggle against distributed locks and the need to keep their nodes’ data in sync\)\.

### Performance

A shared database with consistency guarantees \([ACID](https://en.wikipedia.org/wiki/ACID)\) is likely to lower the total resource consumption compared to one database per service \(as the services don’t need to implement and keep updated [*CQRS views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}}), [MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] of other services’ data\) but it increases latency and it may become the system’s performance bottleneck\. Moreover, by using a shared database services lose the ability to choose the database technologies which best fit their tasks and data\.

Another danger lies with locking records inside the database\. Different services may use different order of tables in transactions, hitting deadlocks in the database engine which show up as transaction timeouts\.

Non\-transactional distributed databases may be very fast when colocated with the services \(see [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\) but the resource consumption becomes very high because of the associated data duplication \(as every instance of each service gets a copy of the entire dataset\) and simultaneous writes may corrupt the data \(cause inconsistencies or merge conflicts\)\.

### Dependencies

Normally, every service depends on the repository\. If the repository does not provide notifications on changes to the data, the services may need to communicate directly, in which case they will also depend on each other through [*choreography*]({{< relref "../part-1--foundations/arranging-communication/choreography.md" >}}) or *mutual* [*orchestration*]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}})\.

<figure style="text-align:center">
<a href="/Dependencies/SharedRepository-1.png" style="outline:none">
<img src="/Dependencies/SharedRepository-1.png" alt="SharedRepository-1" width=100%/>
</a>
</figure>

The dependency on repository technology and a data schema is dangerous for long\-running projects as both of them may need to change sooner or later\. Decoupling the code from the data storage is done with [yet another layer of indirection](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering) which is called a [*Database Abstraction Layer*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(*DAL*\), a *Database Access Layer* \[[POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\], or a *Data Mapper* \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\]\. The DAL, which translates between the data schema and database’s API on one side and the business logic’s SPI on the other side, may reside inside each service or wrap the database:

<figure style="text-align:center">
<a href="/Dependencies/SharedRepository-2.png" style="outline:none">
<img src="/Dependencies/SharedRepository-2.png" alt="SharedRepository-2" width=100%/>
</a>
</figure>

Still, the DAL does not remove shared dependencies and only adds some flexibility\. It seems that there is a peculiar kind of coupling through shared components: if one of the services needs to change the database schema or technology to better suit its needs, it is unable to do so because other components rely on \(and exploit\) the old schema and technology\. Even deploying a second database, private to the service, is often not an option, as there is no convenient way to keep the databases in sync\.

### Applicability

*Shared Repository* is <ins>good</ins> for:

- *Data\-centric domains\.* If most of your domain’s data is used in every subdomain, keeping any part of it private to a single service will be a pain in the system design\. Examples include a [ticket reservation system]({{< relref "../part-1--foundations/arranging-communication/shared-data.md" >}}) and even the minesweeper game\.
- *A scalable service\.* When you run several [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of a service, like in [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}), the instances are likely to be identical and stateless, with the service’s data pushed out to a database shared among the instances\.
- *Huge datasets*\. Sometimes you may need to deal with a lot of data\. It is unwise \(meaning expensive\) to stream and replicate it between your services just for the sake of ensuring their isolation\. Share it instead\. If the data does not fit in an ordinary database, some kind of [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) \(which [was invented to this end](https://en.wikipedia.org/wiki/Space-based_architecture#History)\) may become your friend\.
- *Quick simple projects\.* Don’t over\-engineer if the project won’t live long enough to benefit from its flexibility\. You may also save a buck or two on the storage\.


*Shared Repository* is <ins>bad</ins> for:

- *Quickly evolving, complex projects\.* As everything changes, you just cannot devise a stable schema, while every change of the database schema breaks all the services\.
- *Varied forces and algorithms*\. Different services may require different kinds of databases to work efficiently\.
- *Big data with random writes*\. Your data does not fit on a single server\. If you want to avoid write conflicts, you must keep all the database nodes synchronized, which kills performance\. If you let them all broadcast their changes asynchronously, you get collisions\. You may want to first decouple and [*shard*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) the data as much as possible, and then turn your attention to esoteric databases, specialized caches, and even tailor\-made [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) to get out of the trouble\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Shared%20Repository.png" style="outline:none">
<img src="/Relations/Shared%20Repository.png" alt="Shared Repository" width=100%/>
</a>
</figure>

*Shared Repository*:

- Extends [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), or occasionally [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\.
- Is a part of a [persistent *Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md#persistent-event-log-shared-event-store" >}}) or [*Nanoservices*]({{< relref "../part-2--basic-metapatterns/pipeline.md#function-as-a-service-faas-nanoservices-pipelined" >}})\.
- Is [closely related](https://itnext.io/a-practical-guide-to-modular-monoliths-with-net-59da23c01137) to [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.
- May be implemented by a [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\.


## Variants

*Shared Repository* is a sibling of [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. While a *Middleware* assists direct communication between services \(*shared\-nothing* messaging\), a *Shared Repository* grants them indirect communication through access to an external state \(similar to *shared memory*\) which usually stores all the data for the domain\.

A *Shared Repository* may provide a generic interface \(e\.g\. SQL\) or a custom API \(with a domain\-aware [*Adapter*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) / [*ORM*](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) for the database\)\. The *repository* can be anything ranging from a trivial OS file system or a memory block accessible from all the components to an ordinary database to a [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\-based, distributed [*tuple space*](https://en.wikipedia.org/wiki/Tuple_space):

### Shared Database, Integration Database, Data Domain, Database of Service\-Based Architecture

<figure style="text-align:center">
<a href="/Variants/2/Shared%20Database.png" style="outline:none">
<img src="/Variants/2/Shared%20Database.png" alt="Shared Database" width=93%/>
</a>
</figure>

*Shared Database* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\], [*Integration Database*](https://martinfowler.com/bliki/IntegrationDatabase.html)*,* or *Data Domain* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] is a single database available to several [services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\. The services may subscribe to data change triggers in the database itself or notify each other directly about domain events\. The latter is often the case with [*Service\-Based Architecture*]({{< relref "../part-2--basic-metapatterns/services.md#service-based-architecture" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] which consists of large services dedicated to subdomains\.

### Blackboard

<figure style="text-align:center">
<a href="/Variants/2/Blackboard.png" style="outline:none">
<img src="/Variants/2/Blackboard.png" alt="Blackboard" width=100%/>
</a>
</figure>

[*Blackboard*](https://hillside.net/plop/plop97/Proceedings/lalanda.pdf) \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}}), [POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\] was used for non\-deterministic calculations where several algorithms were concurring and collaborating to gradually build a solution from incomplete inputs\. The *control* \([*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) component schedules the work of several *knowledge sources* \([*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\) which encapsulate algorithms for processing the data stored in the *blackboard* \(*Shared Repository*\)\. This approach has likely been superseded by convolutional neural networks\.

Examples: several use cases are [mentioned on Wikipedia](https://en.wikipedia.org/wiki/Blackboard_system)\.

### Data Grid of Space\-Based Architecture \(SBA\), Replicated Cache, Distributed Cache

<figure style="text-align:center">
<a href="/Variants/2/Data%20Grid.png" style="outline:none">
<img src="/Variants/2/Data%20Grid.png" alt="Data Grid" width=100%/>
</a>
</figure>

The [*Space\-Based Architecture*](https://en.wikipedia.org/wiki/Space-based_architecture) \(*SBA*\) \[[SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sap" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is a [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) \(a [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\-based [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) with at least one [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) per service instance\) which also implements an in\-memory [*tuple space*](https://en.wikipedia.org/wiki/Tuple_space) \(shared dictionary\)\. Although it does not provide a full\-featured database interface it has very good performance, elasticity, and fault tolerance, while some implementations allow for dealing with datasets which are much larger than anything digestible by ordinary databases\. Its drawbacks include write collisions and high operating costs \(huge traffic for data replication and lots of RAM to store the replicas\)\.

The main components of the architecture are:

- *Processing Units* – the [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) with the business logic\. There may be one class of *Processing Units*, making *SBA* look like [*Pool*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \([*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\), or multiple classes, in which case the architecture becomes similar to [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) with a *Shared Database*\.
- *Data Grid* \(*Replicated Cache* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\]\) – a [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\-based in\-memory database\. Each node of the *Data Grid* is co\-located with a single instance of a *Processing Unit*, providing the latter with very fast access to the data\. Changes to the data are replicated across the grid by its virtual *Data Replication Engine* which is usually implemented by every node of the grid\.
- *Persistent Database* – an external database which the *Data Grid* replicates \(caches\)\. Its schema is encapsulated in the *Readers* and *Writers*\.
- *Data Readers* – components that read any data unavailable in the *Data Grid* from the *Persistent Database*\. Most cases see *Readers* employed upon starting the system to upload the entire contents of the database into the memory of the nodes\.
- *Data Writers* – components that replicate the changes done in the *Data Grid* to the persistent storage to assure that no updates are lost if the system is shut down\. There can be a pair of *Reader* and *Writer* per class of *Processing Units* \(subdomain\) or a global pair that processes all read and write requests\.


*SBA* provides nearly perfect scalability \(high read and write throughput as all the data is [cached]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#database-cache-cache-aside" >}})\) and elasticity \(new instances of *Processing Units* are created and initialized very quickly as they copy their data from already running units with no need to access the *Persistent Database*\)\. Though for smaller datasets the entire database is [replicated]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) to every node of the grid \(*Replicated Cache* mode\), *Space\-Based Architecture* also allows the processing of datasets that don’t fit into the memory of a single node by assigning each node a [*shard*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) of the dataset \(*Distributed Cache* mode\)\.

The drawbacks of this architecture include:

- Structural and operational complexity\.
- Very basic dictionary\-like interface of the *tuple space* \(no joins or other complex operations\)\.
- High traffic for data replication among the nodes\.
- Data collisions if multiple clients change the same piece of data simultaneously\.


### Shared Memory

<figure style="text-align:center">
<a href="/Variants/2/Shared%20memory.png" style="outline:none">
<img src="/Variants/2/Shared%20memory.png" alt="Shared memory" width=100%/>
</a>
</figure>

Several actors \(processes, modules, device drivers\) communicate through one or more mutually accessible data structures \(arrays, trees, or dictionaries\)\. Accessing a shared object may require some kind of synchronization \(e\.g\. taking a *mutex*\) or employ [*atomic variables*](https://codescoddler.medium.com/concurrency-made-simple-the-role-of-atomic-variables-8327b9b35023)\. Notwithstanding that communication via *shared memory* is the archenemy of \([*shared\-nothing*](https://www.scylladb.com/glossary/shared-nothing-architecture/)\) messaging it is actually used to implement messaging: high\-load multi\-process systems \(web browsers and high\-frequency trading\) rely on shared memory *mailboxes* for messaging between their [constituent processes]({{< relref "../part-2--basic-metapatterns/services.md#multiple-processes" >}})\.

### Shared File System

<figure style="text-align:center">
<a href="/Variants/2/Shared%20files.png" style="outline:none">
<img src="/Variants/2/Shared%20files.png" alt="Shared files" width=100%/>
</a>
</figure>

As a file system is a kind of shared dictionary, writing and reading files can be used to transfer data between applications\. A [data processing]({{< relref "../part-1--foundations/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}) [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) which stores intermediate results in files benefits from the ability to restart its calculation from the last successful step because files are persistent \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\]\.

### Persistent Event Log, Shared Event Store

<figure style="text-align:center">
<a href="/Variants/2/Shared%20Database%20-%20Event%20Log.png" style="outline:none">
<img src="/Variants/2/Shared%20Database%20-%20Event%20Log.png" alt="Shared Database - Event Log" width=95%/>
</a>
</figure>

A database which stores events \(*event log* for interservice events, *event store* for internal state changes\) can be used as a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}): an event producer writes its events to a topic in the repository while event consumers get notified as soon as a new record appears\.

More details are [available]({{< relref "../part-3--extension-metapatterns/combined-component.md#persistent-event-log-shared-event-store" >}}) in the *Combined Component* chapter\.

### \(inexact\) Stamp Coupling

<figure style="text-align:center">
<a href="/Variants/2/Stamp%20Coupling.png" style="outline:none">
<img src="/Variants/2/Stamp%20Coupling.png" alt="Stamp Coupling" width=96%/>
</a>
</figure>

*Stamp Coupling* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] happens when a single data structure passes through an entire [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}), with separate fields of the data structure targeting individual processing steps\.

A [*choreographed*]({{< relref "../part-1--foundations/arranging-communication/choreography.md" >}}) system with no shared databases does not provide any way to aggregate the data spread over its multiple services\. If we need to collect everything known about a user or purchase, we pass a query message through the system, and every service appends to it whatever it knows of the subject \(just like post offices add their *stamps* to a letter\)\. The unified message becomes a kind of virtual \(temporary\) *Shared Repository* which the services \(*Content Enrichers* according to \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\]\) write to\. This also manifests in the dependencies: all the services [depend on the format of the query message]({{< relref "../part-1--foundations/arranging-communication/choreography.md#dependencies" >}}) as they would on the schema of a *Shared Repository*, instead of depending on each other, as is usual with *Pipelines*\.

## Evolutions

Once a database appears, it is unlikely to go away\. I see the following evolutions to improve performance of the data layer:

- [Shard]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) the database\.


<figure style="text-align:center">
<a href="/Evolutions/2/Shared%20Database_%20Shard.png" style="outline:none">
<img src="/Evolutions/2/Shared%20Database_%20Shard.png" alt="Shared Database: Shard" width=100%/>
</a>
</figure>

- Use [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) for dynamic scalability\.


<figure style="text-align:center">
<a href="/Evolutions/2/Shared%20Database%20to%20Space-Based%20Architecture.png" style="outline:none">
<img src="/Evolutions/2/Shared%20Database%20to%20Space-Based%20Architecture.png" alt="Shared Database to Space-Based Architecture" width=100%/>
</a>
</figure>

- Divide the data into private databases\.


<figure style="text-align:center">
<a href="/Evolutions/2/Shared%20Database%20to%20Services.png" style="outline:none">
<img src="/Evolutions/2/Shared%20Database%20to%20Services.png" alt="Shared Database to Services" width=100%/>
</a>
</figure>

- Deploy specialized databases \([*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


<figure style="text-align:center">
<a href="/Evolutions/2/Shared%20Database%20to%20Polyglot%20Persistence.png" style="outline:none">
<img src="/Evolutions/2/Shared%20Database%20to%20Polyglot%20Persistence.png" alt="Shared Database to Polyglot Persistence" width=100%/>
</a>
</figure>

## Summary

A *Shared Repository* encapsulates a system’s data, allowing for [data\-centric]({{< relref "../part-1--foundations/arranging-communication/shared-data.md" >}}) development and kickstarting [*Service\-Based*]({{< relref "../part-2--basic-metapatterns/services.md#service-based-architecture" >}}) architectures through simplifying interservice interactions\. Its downsides are a frozen data schema and limited performance\.

<nav>

| \<\< [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) | ^ [Part 3\. Extension Metapatterns]({{< relref "../part-3--extension-metapatterns/_index.md" >}}) ^ | [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) \>\> |
| --- | --- | --- |

</nav>



