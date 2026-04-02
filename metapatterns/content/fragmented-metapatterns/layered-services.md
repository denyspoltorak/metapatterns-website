+++
weight = 6
title = "Layered Services"
description = "This chapter discusses layered services, their orchestrated and choreographed variants, and Command-Query Responsibility Segregation (CQRS) systems."
images = ["/diagrams/Web/og/Layered%20Services.png"]
primary_image = "/diagrams/Main/Layered%20Services.png"
[sitemap]
  priority = 0.8
+++

# Layered Services {anchor=false}

<figure>
<a href="/diagrams/Main/Layered%20Services.png">
<picture>
<source srcset="/diagrams/Main/Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Layered%20Services.png" alt="Layered Services, with a legend." loading="lazy" width="1122" height="534" style="width:100%"/>
</picture>
</a>
</figure>

*Cut the cake\.* Divide each service into layers\.

<ins>Structure:</ins> Subdomain services divided into layers\.

<ins>Type:</ins> Implementation of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) or [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}})\.

*Layered Services* is an umbrella architecture for common implementations of systems of [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\. It does not introduce any special features as layers are completely encapsulated by the service which they belong to\. Still, as the services may communicate at different layers, there are a couple of things to learn by exploring the subject matter\.

### Performance

*Layered Services* are similar to [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) performance\-wise: use cases that involve a single service are the fastest, those that need to synchronize states of multiple services are the slowest\.

Remarkable features of *Layered Services* include:

- Independent scaling of layers of the services\. It is common to have multiple [instances]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-load-balanced-services-work-queue-lambdas" >}}) \(with the number varying from service to service and changing dynamically under load\) of the layers that contain business logic while the corresponding data layers \(databases\) are limited to a single instance\.


<figure>
<a href="/diagrams/Performance/Layered%20Services%20-%20sharding.png">
<picture>
<source srcset="/diagrams/Performance/Layered%20Services%20-%20sharding.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layered%20Services%20-%20sharding.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layered%20Services%20-%20sharding.png" alt="The use of scaled stateless services and load balancers in Layered Services." loading="lazy" width="1003" height="423" style="width:100%"/>
</picture>
</a>
</figure>

- The option to establish additional communication channels between lower layers in order to drive [*CQRS*]({{< relref "#command-query-responsibility-segregation-cqrs" >}}) databases \([read/write replicas]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#read-only-replicas" >}}) of the same database\) or [*CQRS Views*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \(cached subsets of data from other services\) \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\]\.


<figure>
<a href="/diagrams/Performance/Layered%20Services%20-%20channels.png">
<picture>
<source srcset="/diagrams/Performance/Layered%20Services%20-%20channels.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layered%20Services%20-%20channels.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layered%20Services%20-%20channels.png" alt="Data streams in Three-Layered Services: from data layer to data layer, from domain layer to data layer, and between two domain-level components." loading="lazy" width="1123" height="403" style="width:100%"/>
</picture>
</a>
</figure>

## Variants

*Layered Services* vary in the number of [layers]({{< relref "../basic-metapatterns/layers.md" >}}) and in the layer through which the [services]({{< relref "../basic-metapatterns/services.md" >}}) communicate:

- In [*Orchestrated Three\-Layered Services*]({{< relref "#orchestrated-three-layered-services" >}}) the application layer of any service may call the same layer in other services\.
- In [*Choreographed Two\-Layered Services*]({{< relref "#choreographed-two-layered-services" >}}) the domain layers of services are assembled into a *Pipeline*\.
- [*Command Query Responsibility Segregation*]({{< relref "#command-query-responsibility-segregation-cqrs" >}}) streams changes from a transactional to an analytical database\.


## Orchestrated Three\-Layered Services 

<figure>
<a href="/diagrams/Variants/3/Three-Layered%20Services.png">
<picture>
<source srcset="/diagrams/Variants/3/Three-Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/Three-Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/Three-Layered%20Services.png" alt="In three-layered services the application layer of every service calls the domain layer of its service and the application layers of other services." loading="lazy" width="923" height="332" style="width:100%"/>
</picture>
</a>
</figure>

Probably the most common backend architecture has [three layers]({{< relref "../basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}): [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}), [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}), and *infrastructure* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\. The application layer [*orchestrates*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) the domain layer\.

If such an architecture is divided into [services]({{< relref "../basic-metapatterns/services.md" >}}), each of them receives a part of every layer, including application, which means that now there are as many *Orchestrators* as services\. Each *Orchestrator* implements the API of its service by integrating \(calling or messaging into\) the domain layer of its service and APIs of other services, which makes all the *Orchestrators* interdependent:

### Dependencies

The upper \([*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}})\) layer of each service orchestrates both its middle \(*domain*\) layer and the upper layers of other services, resulting in [mutual orchestration and interdependencies]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}})\.

<figure>
<a href="/diagrams/Communication/Mutual%20Orchestration%20-%204.png">
<picture>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%204.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%204.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mutual%20Orchestration%20-%204.png" alt="In Layered Services only the application layers of the services are interdependent." loading="lazy" width="1023" height="294" style="width:100%"/>
</picture>
</a>
</figure>

The good thing is that the majority of the code belongs to the domain layer which depends only on its databases\. The bad thing is that changes in the application of one service may affect the application layers of all of the other services\.

### Relations

*Three\-layered services*:

- Implement [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Are derived from [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Have multiple [*Integration \(sub\)Services*]({{< relref "../extension-metapatterns/orchestrator.md#integration-micro-service-application-service" >}}) \([*Orchestrators*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\)\.


### Evolutions

*Orchestrated Layered Services* may become coupled, which is resolved either by merging their layers:

- A part of or the whole [*application layer*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) can be merged into a shared [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\.
- Some or all the [*databases*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) can be united into a [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) or shared as [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Both the *application* and *data* layers can be merged into a [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}})*\.*


<figure>
<a href="/diagrams/Evolutions/3/Three-Layered%20Services%20-%201.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%201.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%201.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Three-Layered%20Services%20-%201.png" alt="Diagrams for Three-Layered Services with partially merged application layer, partially merged databases and shared databases, and a Sandwich." loading="lazy" width="1004" height="814" style="width:84%"/>
</picture>
</a>
</figure>

or by building derived datasets:

- A [*CQRS View*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) inside a service aggregates any events from other services which its owner is interested in\.
- A dedicated [*Query Service*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) captures the whole system’s state by subscribing to events from all the services\.


<figure>
<a href="/diagrams/Evolutions/3/Three-Layered%20Services%20-%202.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%202.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Three-Layered%20Services%20-%202.png" alt="Diagrams for Three-Layered Services employing CQRS views and a Query Service." loading="lazy" width="1143" height="494" style="width:100%"/>
</picture>
</a>
</figure>

If the services become too large:

- The middle layer can be split into [*Sandwiches*]({{< relref "../extension-metapatterns/sandwich.md" >}}) or [*Cells*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\.


<figure>
<a href="/diagrams/Evolutions/3/Three-Layered%20Services%20-%203.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%203.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Three-Layered%20Services%20-%203.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Three-Layered%20Services%20-%203.png" alt="The domain layer of a large three-layered service is split into sub-subdomain components, resulting in a Sandwich Cell." loading="lazy" width="1503" height="334" style="width:100%"/>
</picture>
</a>
</figure>

## Choreographed Two\-Layered Services

<figure>
<a href="/diagrams/Variants/3/Two-Layered%20Services.png">
<picture>
<source srcset="/diagrams/Variants/3/Two-Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/Two-Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/Two-Layered%20Services.png" alt="The domain-level components of two-layered services participate in multiple pipelines and access their service's databases." loading="lazy" width="963" height="245" style="width:100%"/>
</picture>
</a>
</figure>

If there is no [*orchestration*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}), there is no role for the [*application* layer]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}})\. [*Choreographed*]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) systems are made up of services that implement individual steps of request processing\. The sequence of actions \(*integration logic*\) which three\-layered systems put in the [*Orchestrators*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) now moves to the graph of *event channels* between the services\. This means that with choreography the high\-level part of the business logic \(use cases\) exists outside of the code of the constituent services\.

### Dependencies

Dependencies are identical to those of a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) or [*choreographed*]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) except that each service also depends on its database\.

### Relations

*Two\-layered services*:

- Implement [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.
- Are derived from [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.


### Evolutions

If *Choreographed Layered Services* become coupled:

- The *business logic* of two or more services can be merged together, resulting in [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Some databases can be united into a [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) or shared as [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.


<figure>
<a href="/diagrams/Evolutions/3/Two-Layered%20Services%20-%201.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%201.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%201.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Two-Layered%20Services%20-%201.png" alt="Diagrams for Two-Layered Services with partially merged domain layer, partially merged databases, and shared databases." loading="lazy" width="1324" height="324" style="width:100%"/>
</picture>
</a>
</figure>

[*CQRS Views*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) or [*Query Services*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) are also an option:

<figure>
<a href="/diagrams/Evolutions/3/Two-Layered%20Services%20-%202.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%202.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Two-Layered%20Services%20-%202.png" alt="Diagrams for Two-Layered Services employing CQRS views and a Query Service." loading="lazy" width="1104" height="404" style="width:100%"/>
</picture>
</a>
</figure>

An overgrown service can be:

- Split in two


<figure>
<a href="/diagrams/Evolutions/3/Two-Layered%20Services%20-%203.png">
<picture>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%203.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/Two-Layered%20Services%20-%203.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/Two-Layered%20Services%20-%203.png" alt="The domain layer of a large two-layered service is split in half." loading="lazy" width="1503" height="301" style="width:100%"/>
</picture>
</a>
</figure>

## [Command Query Responsibility Segregation]({{< relref "../extension-metapatterns/sandwich.md#command-query-responsibility-segregation-cqrs" >}}) \(CQRS\)

<figure>
<a href="/diagrams/Variants/3/CQRS.png">
<picture>
<source srcset="/diagrams/Variants/3/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/CQRS.png" alt="Write requests from a client go to the write backend and OLTP database which feeds OLAP databases. Read requests go to the scaled read backend and the scaled OLAP database." loading="lazy" width="1007" height="324" style="width:100%"/>
</picture>
</a>
</figure>

*Command Query Responsibility Segregation* \(*CQRS*\) \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}}), [LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\] is, essentially, the division of a [layered]({{< relref "../basic-metapatterns/layers.md" >}}) application or a service into two \(rarely more\) [services]({{< relref "../basic-metapatterns/services.md" >}}), one of which is responsible for write access \(handling *commands*\) to the domain data while the other\(s\) deal with read access \(*queries*\), thus [creating]({{< relref "../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) a data [*pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) \(see the diagram below\)\. Such an architecture makes sense when the write and read operations don’t rely on a common vision \(*model*\) of the domain, for example, writes are individual changes \([*OLTP*](https://en.wikipedia.org/wiki/Online_transaction_processing)\) that require cross\-checks and validation of input while reads show aggregated data \([*OLAP*](https://en.wikipedia.org/wiki/Online_analytical_processing)\) and may take long time to complete \(meaning that [*forces*]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}) for the read and write paths differ\)\. If there is nothing to share in the code, why not separate the implementations?

<figure>
<a href="/diagrams/Variants/3/CQRS%20-%20pipeline%20view.png">
<picture>
<source srcset="/diagrams/Variants/3/CQRS%20-%20pipeline%20view.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/CQRS%20-%20pipeline%20view.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/CQRS%20-%20pipeline%20view.png" alt="In CQRS data streams from the client to the write backend, then to the OLTP database, to the OLAP database, to the read backend and, finally, returns to the client." loading="lazy" width="1003" height="323" style="width:100%"/>
</picture>
</a>
</figure>

This separation brings in the pros and cons of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}): commands and queries may differ in technologies \(including database schemas or even types\), forces, and teams at the expense of [consistency](https://en.wikipedia.org/wiki/Eventual_consistency) \(database replication delay\) and increasing the system’s complexity\. In addition, for read\-heavy applications the read database\(s\) is easy to scale\.

*CQRS* has several variations:

- The database may be shared, commands and queries may use dedicated databases, or the read service may maintain a [*Memory Image*](https://martinfowler.com/bliki/MemoryImage.html) / [*Materialized View*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}) fed by events from the write service \(as in other kinds of *Layered Services*\)\.
- Data [replication]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) may be implemented as a [*pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) between the databases \(based on nightly snapshots or [log\-based replication](https://www.dremio.com/wiki/log-based-replication/)\) or a [direct event feed](https://martinfowler.com/bliki/EagerReadDerivation.html) from the OLTP code to the OLAP database\.


<figure>
<a href="/diagrams/Variants/3/CQRS%20-%20subtypes.png">
<picture>
<source srcset="/diagrams/Variants/3/CQRS%20-%20subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/3/CQRS%20-%20subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/3/CQRS%20-%20subtypes.png" alt="In CQRS the OLAP databases receive data from the OLTP database or from events originating in the write backend. Alternatively, the read and write backends may share a database." loading="lazy" width="1303" height="446" style="width:100%"/>
</picture>
</a>
</figure>

It is noteworthy that while ordinary *Layered Services* usually communicate through their upper\-level components that drive use cases, a *CQRS* system is held together by spreading data changes through its lowest layer\.

Examples: Martin Fowler has a [short article](https://martinfowler.com/bliki/CQRS.html) and Microsoft a [longer one](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)\.

### Dependencies

Each backend depends on its database \(its technology and schema\)\. The OLTP to OLAP data replication requires an additional dependency that corresponds to the way the replication is implemented:

<figure>
<a href="/diagrams/Dependencies/CQRS.png">
<picture>
<source srcset="/diagrams/Dependencies/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/CQRS.png" alt="In CQRS each service depends on its database while the OLAP database depends on the source of its event feed." loading="lazy" width="1243" height="326" style="width:100%"/>
</picture>
</a>
</figure>

### Relations

*CQRS*:

- Implements [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) \(a whole *system* or a single [*service*]({{< relref "../basic-metapatterns/services.md" >}})\)\.
- Is derived from [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.
- Is a development of [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.


### Evolutions

- You will usually need a [*Reverse Proxy*]({{< relref "../extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}) or an [*API Gateway*]({{< relref "../extension-metapatterns/proxy.md#api-gateway" >}}) to segregate commands from queries\.
- If the commands and queries become intermixed, the business logic can be merged together but the databases are left separate, resulting in [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Both read and write backends can be split into [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) or [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) \(yielding [*Cells*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\)\.
- Applying [*Space\-Based Architecture*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) may further improve performance\.
- Multiple schemas or even kinds of OLAP databases can be used simultaneously \([*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


<figure>
<a href="/diagrams/Evolutions/3/CQRS.png">
<picture>
<source srcset="/diagrams/Evolutions/3/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/3/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/3/CQRS.png" alt="Diagrams of CQRS behind an API Gateway, with a single backend, with multiple OLAP databases, with layered backends, Cells for backends, and Data Grid for a database." loading="lazy" width="1284" height="1023" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

*Layered Services* is an umbrella pattern that conjoins:

- *Three\-Layered Services* where each service [*orchestrates*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) other services\.
- *Two\-Layered Services* that form a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.
- *CQRS* that separates read and write request processing paths\.
