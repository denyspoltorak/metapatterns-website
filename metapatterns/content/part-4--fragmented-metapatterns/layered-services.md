+++
weight = 6
title = "Layered Services"
+++

# Layered Services

<p align="center">
<img src="/Main/Layered Services.png" alt="Layered Services" width=100%/>
</p>

*Cut the cake\.* Divide each service into layers\.

<ins>Variants:</ins> 

- [Orchestrated]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}) Three\-Layered Services,
- \(*Pipelined*\) [Choreographed]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}) Two\-Layered Services,
- \(*Pipelined*\) Command Query Responsibility Segregation \(CQRS\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\]\.


<ins>Structure:</ins> Subdomain services divided into layers\.

<ins>Type:</ins> Implementation of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) or [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), correspondingly\.

*Layered Services* is an umbrella architecture for common implementations of systems of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\. It does not introduce any special features as layers are completely encapsulated by the service which they belong to\. Still, as the services may communicate at different layers, there are a couple of things to learn by exploring the subject matter\.

### Performance

*Layered Services* are similar to [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) performance\-wise: use cases that involve a single service are the fastest, those that need to synchronize states of multiple services are the slowest\.

Remarkable features of *Layered Services* include:

- Independent scaling of layers of the services\. It is common to have multiple [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \(with the number varying from service to service and changing dynamically under load\) of the layers that contain business logic while the corresponding data layers \(databases\) are limited to a single instance\.


<p align="center">
<img src="/Performance/Layered Services - sharding.png" alt="Layered Services - sharding" width=100%/>
</p>

- The option to establish additional communication channels between lower layers in order to drive [*CQRS*]({{< relref "#command-query-responsibility-segregation-cqrs" >}}) databases \([read/write replicas]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of the same database\) or [*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \(cached subsets of data from other services\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\.


<p align="center">
<img src="/Performance/Layered Services - channels.png" alt="Layered Services - channels" width=100%/>
</p>

## Variants

*Layered Services* vary in the number of [layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and in the layer through which the [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}) communicate:

## Orchestrated Three\-Layered Services 

<p align="center">
<img src="/Variants/3/Three-Layered Services.png" alt="Three-Layered Services" width=100%/>
</p>

Probably the most common backend architecture has [three layers]({{< relref "../part-2--basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}): *application*, *domain*, and *infrastructure* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\. The application layer [*orchestrates*]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}) the domain layer\.

If such an architecture is divided into [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), each of them receives a part of every layer, including application, which means that now there are as many *Orchestrators* as services\. Each *Orchestrator* implements the API of its service by integrating \(calling or messaging into\) the domain layer of its service and APIs of other services, which makes all the *Orchestrators* interdependent:

### Dependencies

The upper \(application\) layer of each service orchestrates both its middle \(domain\) layer and the upper layers of other services, resulting in [mutual orchestration and interdependencies]({{< relref "../part-1--foundations/arranging-communication.md#mutual-orchestration" >}})\.

<p align="center">
<img src="/Communication/Mutual Orchestration - 4.png" alt="Mutual Orchestration - 4" width=100%/>
</p>

The good thing is that the majority of the code belongs to the domain layer which depends only on its databases\. The bad thing is that changes in the application of one service may affect the application layers of all of the other services\.

### Relations

*Three\-layered services*:

- Implement [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.
- Are derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.
- Have multiple [*Integration \(sub\)Services*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#integration-micro-service-application-service" >}}) \([*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\)\.


### Evolutions

*Orchestrated Layered Services* may become coupled, which is resolved either by merging their layers:

- A part of or the whole application layer can be merged into a shared [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.
- Some or all the *databases* can be united into a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) or shared as [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.


<p align="center">
<img src="/Evolutions/3/Three-Layered Services - 1.png" alt="Three-Layered Services - 1" width=100%/>
</p>

or by building derived datasets:

- A [*CQRS View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] inside a service aggregates any events from other services which its owner is interested in\.
- A dedicated [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] captures the whole system’s state by subscribing to events from all the services\.


<p align="center">
<img src="/Evolutions/3/Three-Layered Services - 2.png" alt="Three-Layered Services - 2" width=100%/>
</p>

If the services become too large:

- The middle layer can be split into [*Cells*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.


<p align="center">
<img src="/Evolutions/3/Three-Layered Services - 3.png" alt="Three-Layered Services - 3" width=100%/>
</p>

## Choreographed Two\-Layered Services

<p align="center">
<img src="/Variants/3/Two-Layered Services.png" alt="Two-Layered Services" width=99%/>
</p>

If there is no [*orchestration*]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}), there is no role for the *application* layer\. [*Choreographed*]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}) systems are made up of services that implement individual steps of request processing\. The sequence of actions \(*integration logic*\) which three\-layered systems put in the [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) now moves to the graph of *event channels* between the services\. This means that with choreography the high\-level part of the business logic \(use cases\) exists outside of the code of the constituent services\.

### Dependencies

Dependencies are identical to those of a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) or [*choreographed*]({{< relref "../part-1--foundations/arranging-communication.md#choreography" >}}) [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) except that each service also depends on its database\.

### Relations

*Two\-layered services*:

- Implement [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.
- Are derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.


### Evolutions

If *Choreographed Layered Services* become coupled:

- The *business logic* of two or more services can be merged together, resulting in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Some databases can be united into a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) or shared as [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.


<p align="center">
<img src="/Evolutions/3/Two-Layered Services - 1.png" alt="Two-Layered Services - 1" width=100%/>
</p>

[*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] or [*Query Services*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] are also an option:

<p align="center">
<img src="/Evolutions/3/Two-Layered Services - 2.png" alt="Two-Layered Services - 2" width=100%/>
</p>

An overgrown service can be:

- Split in two


<p align="center">
<img src="/Evolutions/3/Two-Layered Services - 3.png" alt="Two-Layered Services - 3" width=100%/>
</p>

## Command Query Responsibility Segregation \(CQRS\)

<p align="center">
<img src="/Variants/3/CQRS.png" alt="CQRS" width=100%/>
</p>

*Command Query Responsibility Segregation* \(*CQRS*\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] is, essentially, the division of a [layered]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) application or a service into two \(rarely more\) [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), one of which is responsible for write access \(handling *commands*\) to the domain data while the other\(s\) deal with read access \(*queries*\), thus [creating]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md#pipelines-in-architectural-patterns" >}}) a data [*pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \(see the diagram below\)\. Such an architecture makes sense when the write and read operations don’t rely on a common vision \(*model*\) of the domain, for example, writes are individual changes \([*OLTP*](https://en.wikipedia.org/wiki/Online_transaction_processing)\) that require cross\-checks and validation of input while reads show aggregated data \([*OLAP*](https://en.wikipedia.org/wiki/Online_analytical_processing)\) and may take long time to complete \(meaning that [*forces*]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md" >}}) for the read and write paths differ\)\. If there is nothing to share in the code, why not separate the implementations?

<p align="center">
<img src="/Variants/3/CQRS - pipeline view.png" alt="CQRS - pipeline view" width=100%/>
</p>

This separation brings in the pros and cons of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}): commands and queries may differ in technologies \(including database schemas or even types\), forces, and teams at the expense of [consistency](https://en.wikipedia.org/wiki/Eventual_consistency) \(database replication delay\) and increasing the system’s complexity\. In addition, for read\-heavy applications the read database\(s\) is easy to scale\.

*CQRS* has several variations:

- The database may be shared, commands and queries may use dedicated databases, or the read service may maintain a [*Memory Image*](https://martinfowler.com/bliki/MemoryImage.html) / [*Materialized View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}) \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] fed by events from the write service \(as in other kinds of *Layered Services*\)\.
- Data [replication]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) may be implemented as a [*pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) between the databases \(based on nightly snapshots or [log\-based replication](https://www.dremio.com/wiki/log-based-replication/)\) or a [direct event feed](https://martinfowler.com/bliki/EagerReadDerivation.html) from the OLTP code to the OLAP database\.


<p align="center">
<img src="/Variants/3/CQRS - subtypes.png" alt="CQRS - subtypes" width=100%/>
</p>

It is noteworthy that while ordinary *Layered Services* usually communicate through their upper\-level components that drive use cases, a *CQRS* system is held together by spreading data changes through its lowest layer\.

Examples: Martin Fowler has a [short article](https://martinfowler.com/bliki/CQRS.html) and Microsoft a [longer one](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)\.

### Dependencies

Each backend depends on its database \(its technology and schema\)\. The OLTP to OLAP data replication requires an additional dependency that corresponds to the way the replication is implemented:

<p align="center">
<img src="/Dependencies/CQRS.png" alt="CQRS" width=100%/>
</p>

### Relations

*CQRS*:

- Implements [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) \(a whole *system* or a single [*service*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\)\.
- Is derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.
- Is a development of [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.


### Evolutions

- You will usually need a [*Reverse Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}) or an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#api-gateway" >}}) to segregate commands from queries\.
- If the commands and queries become intermixed, the business logic can be merged together but the databases are left separate, resulting in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Both read and write backends can be split into [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) or [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) \(yielding [*Cells*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\)\.
- Applying [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) may further improve performance\.
- Multiple schemas or even kinds of OLAP databases can be used simultaneously \([*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


<p align="center">
<img src="/Evolutions/3/CQRS.png" alt="CQRS" width=100%/>
</p>

## Summary

*Layered Services* is an umbrella pattern that conjoins:

- *Three\-Layered Services* where each service [*orchestrates*]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}) other services\.
- *Two\-Layered Services* that form a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.
- *CQRS* that separates read and write request processing paths\.


<nav>

| \<\< [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) \>\> |
| --- | --- | --- |

</nav>



