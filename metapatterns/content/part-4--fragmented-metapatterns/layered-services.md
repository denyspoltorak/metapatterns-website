+++
weight = 6
title = "Layered Services"
description = Layered Services may orchestrate each other, rely on choreography, or make a CQRS system. The communication between services happens at different layers.
+++

# Layered Services

<figure style="text-align:center">
<a href="/Main/Layered%20Services.png" style="outline:none">
<img src="/Main/Layered%20Services.png" alt="Layered Services" style="width:100%"/>
</a>
</figure>

*Cut the cake\.* Divide each service into layers\.

<ins>Variants:</ins> 

- [Orchestrated]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}}) Three\-Layered Services,
- \(*Pipelined*\) [Choreographed]({{< relref "../part-1--foundations/arranging-communication/choreography.md" >}}) Two\-Layered Services,
- \(*Pipelined*\) Command Query Responsibility Segregation \(CQRS\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\]\.


<ins>Structure:</ins> Subdomain services divided into layers\.

<ins>Type:</ins> Implementation of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) or [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), correspondingly\.

*Layered Services* is an umbrella architecture for common implementations of systems of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\. It does not introduce any special features as layers are completely encapsulated by the service which they belong to\. Still, as the services may communicate at different layers, there are a couple of things to learn by exploring the subject matter\.

### Performance

*Layered Services* are similar to [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) performance\-wise: use cases that involve a single service are the fastest, those that need to synchronize states of multiple services are the slowest\.

Remarkable features of *Layered Services* include:

- Independent scaling of layers of the services\. It is common to have multiple [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) \(with the number varying from service to service and changing dynamically under load\) of the layers that contain business logic while the corresponding data layers \(databases\) are limited to a single instance\.


<figure style="text-align:center">
<a href="/Performance/Layered%20Services%20-%20sharding.png" style="outline:none">
<img src="/Performance/Layered%20Services%20-%20sharding.png" alt="Layered Services - sharding" style="width:100%"/>
</a>
</figure>

- The option to establish additional communication channels between lower layers in order to drive [*CQRS*]({{< relref "#command-query-responsibility-segregation-cqrs" >}}) databases \([read/write replicas]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of the same database\) or [*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \(cached subsets of data from other services\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\.


<figure style="text-align:center">
<a href="/Performance/Layered%20Services%20-%20channels.png" style="outline:none">
<img src="/Performance/Layered%20Services%20-%20channels.png" alt="Layered Services - channels" style="width:100%"/>
</a>
</figure>

## Variants

*Layered Services* vary in the number of [layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and in the layer through which the [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}) communicate:

## Orchestrated Three\-Layered Services 

<figure style="text-align:center">
<a href="/Variants/3/Three-Layered%20Services.png" style="outline:none">
<img src="/Variants/3/Three-Layered%20Services.png" alt="Three-Layered Services" style="width:100%"/>
</a>
</figure>

Probably the most common backend architecture has [three layers]({{< relref "../part-2--basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}): *application*, *domain*, and *infrastructure* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\. The application layer [*orchestrates*]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}}) the domain layer\.

If such an architecture is divided into [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), each of them receives a part of every layer, including application, which means that now there are as many *Orchestrators* as services\. Each *Orchestrator* implements the API of its service by integrating \(calling or messaging into\) the domain layer of its service and APIs of other services, which makes all the *Orchestrators* interdependent:

### Dependencies

The upper \(application\) layer of each service orchestrates both its middle \(domain\) layer and the upper layers of other services, resulting in [mutual orchestration and interdependencies]({{< relref "../part-1--foundations/arranging-communication/orchestration.md#mutual-orchestration" >}})\.

<figure style="text-align:center">
<a href="/Communication/Mutual%20Orchestration%20-%204.png" style="outline:none">
<img src="/Communication/Mutual%20Orchestration%20-%204.png" alt="Mutual Orchestration - 4" style="width:100%"/>
</a>
</figure>

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


<figure style="text-align:center">
<a href="/Evolutions/3/Three-Layered%20Services%20-%201.png" style="outline:none">
<img src="/Evolutions/3/Three-Layered%20Services%20-%201.png" alt="Three-Layered Services - 1" style="width:100%"/>
</a>
</figure>

or by building derived datasets:

- A [*CQRS View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] inside a service aggregates any events from other services which its owner is interested in\.
- A dedicated [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] captures the whole system’s state by subscribing to events from all the services\.


<figure style="text-align:center">
<a href="/Evolutions/3/Three-Layered%20Services%20-%202.png" style="outline:none">
<img src="/Evolutions/3/Three-Layered%20Services%20-%202.png" alt="Three-Layered Services - 2" style="width:100%"/>
</a>
</figure>

If the services become too large:

- The middle layer can be split into [*Cells*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/3/Three-Layered%20Services%20-%203.png" style="outline:none">
<img src="/Evolutions/3/Three-Layered%20Services%20-%203.png" alt="Three-Layered Services - 3" style="width:100%"/>
</a>
</figure>

## Choreographed Two\-Layered Services

<figure style="text-align:center">
<a href="/Variants/3/Two-Layered%20Services.png" style="outline:none">
<img src="/Variants/3/Two-Layered%20Services.png" alt="Two-Layered Services" style="width:99%"/>
</a>
</figure>

If there is no [*orchestration*]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}}), there is no role for the *application* layer\. [*Choreographed*]({{< relref "../part-1--foundations/arranging-communication/choreography.md" >}}) systems are made up of services that implement individual steps of request processing\. The sequence of actions \(*integration logic*\) which three\-layered systems put in the [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) now moves to the graph of *event channels* between the services\. This means that with choreography the high\-level part of the business logic \(use cases\) exists outside of the code of the constituent services\.

### Dependencies

Dependencies are identical to those of a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) or [*choreographed*]({{< relref "../part-1--foundations/arranging-communication/choreography.md" >}}) [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) except that each service also depends on its database\.

### Relations

*Two\-layered services*:

- Implement [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.
- Are derived from [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.


### Evolutions

If *Choreographed Layered Services* become coupled:

- The *business logic* of two or more services can be merged together, resulting in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Some databases can be united into a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) or shared as [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/3/Two-Layered%20Services%20-%201.png" style="outline:none">
<img src="/Evolutions/3/Two-Layered%20Services%20-%201.png" alt="Two-Layered Services - 1" style="width:100%"/>
</a>
</figure>

[*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] or [*Query Services*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] are also an option:

<figure style="text-align:center">
<a href="/Evolutions/3/Two-Layered%20Services%20-%202.png" style="outline:none">
<img src="/Evolutions/3/Two-Layered%20Services%20-%202.png" alt="Two-Layered Services - 2" style="width:100%"/>
</a>
</figure>

An overgrown service can be:

- Split in two


<figure style="text-align:center">
<a href="/Evolutions/3/Two-Layered%20Services%20-%203.png" style="outline:none">
<img src="/Evolutions/3/Two-Layered%20Services%20-%203.png" alt="Two-Layered Services - 3" style="width:100%"/>
</a>
</figure>

## Command Query Responsibility Segregation \(CQRS\)

<figure style="text-align:center">
<a href="/Variants/3/CQRS.png" style="outline:none">
<img src="/Variants/3/CQRS.png" alt="CQRS" style="width:100%"/>
</a>
</figure>

*Command Query Responsibility Segregation* \(*CQRS*\) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] is, essentially, the division of a [layered]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) application or a service into two \(rarely more\) [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}), one of which is responsible for write access \(handling *commands*\) to the domain data while the other\(s\) deal with read access \(*queries*\), thus [creating]({{< relref "../part-6--analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) a data [*pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \(see the diagram below\)\. Such an architecture makes sense when the write and read operations don’t rely on a common vision \(*model*\) of the domain, for example, writes are individual changes \([*OLTP*](https://en.wikipedia.org/wiki/Online_transaction_processing)\) that require cross\-checks and validation of input while reads show aggregated data \([*OLAP*](https://en.wikipedia.org/wiki/Online_analytical_processing)\) and may take long time to complete \(meaning that [*forces*]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md" >}}) for the read and write paths differ\)\. If there is nothing to share in the code, why not separate the implementations?

<figure style="text-align:center">
<a href="/Variants/3/CQRS%20-%20pipeline%20view.png" style="outline:none">
<img src="/Variants/3/CQRS%20-%20pipeline%20view.png" alt="CQRS - pipeline view" style="width:100%"/>
</a>
</figure>

This separation brings in the pros and cons of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}): commands and queries may differ in technologies \(including database schemas or even types\), forces, and teams at the expense of [consistency](https://en.wikipedia.org/wiki/Eventual_consistency) \(database replication delay\) and increasing the system’s complexity\. In addition, for read\-heavy applications the read database\(s\) is easy to scale\.

*CQRS* has several variations:

- The database may be shared, commands and queries may use dedicated databases, or the read service may maintain a [*Memory Image*](https://martinfowler.com/bliki/MemoryImage.html) / [*Materialized View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}) \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddia" >}})\] fed by events from the write service \(as in other kinds of *Layered Services*\)\.
- Data [replication]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) may be implemented as a [*pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) between the databases \(based on nightly snapshots or [log\-based replication](https://www.dremio.com/wiki/log-based-replication/)\) or a [direct event feed](https://martinfowler.com/bliki/EagerReadDerivation.html) from the OLTP code to the OLAP database\.


<figure style="text-align:center">
<a href="/Variants/3/CQRS%20-%20subtypes.png" style="outline:none">
<img src="/Variants/3/CQRS%20-%20subtypes.png" alt="CQRS - subtypes" style="width:100%"/>
</a>
</figure>

It is noteworthy that while ordinary *Layered Services* usually communicate through their upper\-level components that drive use cases, a *CQRS* system is held together by spreading data changes through its lowest layer\.

Examples: Martin Fowler has a [short article](https://martinfowler.com/bliki/CQRS.html) and Microsoft a [longer one](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)\.

### Dependencies

Each backend depends on its database \(its technology and schema\)\. The OLTP to OLAP data replication requires an additional dependency that corresponds to the way the replication is implemented:

<figure style="text-align:center">
<a href="/Dependencies/CQRS.png" style="outline:none">
<img src="/Dependencies/CQRS.png" alt="CQRS" style="width:100%"/>
</a>
</figure>

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


<figure style="text-align:center">
<a href="/Evolutions/3/CQRS.png" style="outline:none">
<img src="/Evolutions/3/CQRS.png" alt="CQRS" style="width:100%"/>
</a>
</figure>

## Summary

*Layered Services* is an umbrella pattern that conjoins:

- *Three\-Layered Services* where each service [*orchestrates*]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}}) other services\.
- *Two\-Layered Services* that form a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.
- *CQRS* that separates read and write request processing paths\.


<nav>

| \<\< [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) \>\> |
| --- | --- | --- |

</nav>



