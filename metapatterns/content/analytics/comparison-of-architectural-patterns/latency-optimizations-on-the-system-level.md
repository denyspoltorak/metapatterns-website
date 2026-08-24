+++
weight = 6
title = "Latency optimizations on the system level"
description = "Architectural level latency optimizations co-locate or bypass components, preprocess and preload data, and rely on non-blocking access or request hedging."
images = ["/diagrams/Web/og/Latency.png"]
primary_image = "/diagrams/Performance/Plugins-injection.png"
[sitemap]
  priority = 0.5
+++

# Latency optimizations on the system level {anchor=false}

Identifying and optimizing [hot spots](https://en.wikipedia.org/wiki/Hot_spot_(computer_programming)) in the code may boost the performance of your system, but the improvement is unlikely to be drastic unless the code was poorly written initially, a condition known as [*premature pessimization*](https://elegantchaos.com/2014/10/27/premature-pessimization-and-the-like.html)\. Conversely, a single system\-level optimization, if successful,  can remove unnecessary network calls and file access operations from the hot path, sometimes both improving latency by an order of magnitude and lowering the load on the main system\.

## Shortcutting

The most obvious way to speed up answering a request or reacting to an event is to skip some of the involved system components\.

<figure>
<a href="/diagrams/Conclusion/Latency-EarlyResponse.png">
<picture>
<source srcset="/diagrams/Conclusion/Latency-EarlyResponse.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Latency-EarlyResponse.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Latency-EarlyResponse.png" alt="In a rate limiter, response cache, or a pipeline the component which receives a client request may respond immediately." loading="lazy" width="1003" height="403" style="width:100%"/>
</picture>
</a>
</figure>

In an *early response* one of the first components to receive the input may immediately generate an answer or block the input from reaching the rest of the system:

- A [*Firewall* or *Rate Limiter*]({{< relref "../../extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}) blocks disallowed requests\. It may send a pre\-generated response or ignore the input\.
- A [*Response Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) may answer a client request without consulting the underlying business logic if it has recently seen an identical request and remembered the response\.
- The first component of a [*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}}) may [immediately notify the client]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md#early-response" >}}) that its request has been accepted and then push the request down the pipeline for actual processing\.


It’s hard to imagine anything that responds faster than such systems\.

<figure>
<a href="/diagrams/Conclusion/Latency-Bypass.png">
<picture>
<source srcset="/diagrams/Conclusion/Latency-Bypass.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Latency-Bypass.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Latency-Bypass.png" alt="A hierarchical control system, Model-View-Controller, and and an OS with kernel bypass featuring early response." loading="lazy" width="1183" height="403" style="width:100%"/>
</picture>
</a>
</figure>

It is also possible to *bypass the main system*, either completely or for a rapid provisional response:

- A [*hierarchical*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}) [*control system*]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) often relies on its low\-level nodes to react rapidly to incoming events while its high\-level business logic analyzes the situation and chooses the best long\-term strategy\.
- Various kinds of [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/_index.md#hexagonal-architecture" >}}) and [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) tend to bypass their core components as a performance optimization\.
  - This is why there is the communication path between the view and controller in [*Model\-View\-Controller*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}})\.
  - Low latency systems often rely on [DPDK](https://en.wikipedia.org/wiki/Data_Plane_Development_Kit) which maps network packets directly into the application’s memory\.


<figure>
<a href="/diagrams/Conclusion/Latency-Omit.png">
<picture>
<source srcset="/diagrams/Conclusion/Latency-Omit.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Latency-Omit.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Latency-Omit.png" alt="In a half-proxy or open orchestrator a system layer may be omitted. A telephone server leaves conversation after the phones establish a direct connection." loading="lazy" width="1063" height="403" style="width:100%"/>
</picture>
</a>
</figure>

Finally, one of the system components may be *omitted* from interactions in some scenarios or once it has fulfilled its role:

- A [*Half\-Proxy*]({{< relref "../../extension-metapatterns/proxy.md#half-proxy" >}}) connects the client to a matching server and then steps out of the way\.
- An [*open layer*]({{< relref "../../basic-metapatterns/layers.md#dependencies" >}}) \(or [*open orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md#open-or-relaxed" >}})\) may be used in processing some requests but omitted for others\. Likewise, [*Layered Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md#layered" >}}) relies on bypassing the complex orchestration logic for the most common use cases\.
- Telephony servers and [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) exist in general to help their clients find each other and establish a direct connection\.


## Co\-locating

Each network hop causes delay, therefore we should co\-locate as many components as possible:

<figure>
<a href="/diagrams/Conclusion/Latency-Colocate.png">
<picture>
<source srcset="/diagrams/Conclusion/Latency-Colocate.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Latency-Colocate.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Latency-Colocate.png" alt="An ambassador sharding proxy, mesh nodes in sidecars, and an actor framework optimize latency by co-locating system components." loading="lazy" width="1043" height="423" style="width:100%"/>
</picture>
</a>
</figure>

- The most common example is the [*frontend*]({{< relref "../../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) [*tier*]({{< relref "../../basic-metapatterns/layers.md#three-tier-architecture" >}}) which runs on a user device and thus provides for seamless user interaction\.
- An [*Ambassador*]({{< relref "../../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) is co\-located with a client application but acts on the system’s behalf\. An example is a [*Sharding Proxy*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) which lets the client software connect directly to the [*shard*]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) which contains the client’s data\.
- A [*Sidecar*]({{< relref "../../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) co\-locates generic code with a system’s service\. Sidecars are used in [*Service Mesh*]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}) and [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/sandwich.md#space-based-architecture" >}}) to reduce latency between the business logic and the distributed communication infrastructure\.
- Distributed [*Microkernels*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) invest in co\-location optimizations:
  - [*Actor frameworks*]({{< relref "../../implementation-metapatterns/microkernel.md#virtualizer-hypervisor-container-orchestrator-distributed-runtime" >}}) tend to move the [actors]({{< relref "../../basic-metapatterns/services.md#class-like-actors" >}}) between hosts to ensure that intensely interacting actors run on the same host\.
  - In [*AUTOSAR*]({{< relref "../../implementation-metapatterns/microkernel.md#autosar-classic-platform" >}}) it is preferable to run an application and the services which it uses on the same chip\.
- [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) is the pattern in which [everything is co\-located]({{< relref "../../analytics/ambiguous-patterns.md#monolith" >}}), therefore it can have very low latency if properly implemented\.


## Preloading and preprocessing

If you have all the data you need in the right place and right format, you can use it right away:

<figure>
<a href="/diagrams/Conclusion/Latency-Preloading.png">
<picture>
<source srcset="/diagrams/Conclusion/Latency-Preloading.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Latency-Preloading.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Latency-Preloading.png" alt="A Space-Based Architecture, CQRS, and CQRS view." loading="lazy" width="943" height="723" style="width:100%"/>
</picture>
</a>
</figure>

- [*Actors*]({{< relref "../../basic-metapatterns/shards.md#temporary-state-create-on-demand-actors" >}}) and [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/sandwich.md#space-based-architecture" >}}) keep the data in operating memory next to the code which uses it\. They are blazingly fast\.
- Most implementations of [*Response Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) keep the data in memory as well\.
- Various kinds of [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) stream changes to a derived database specialized in queries or analytics:
  - [*CQRS*]({{< relref "../../extension-metapatterns/sandwich.md#command-query-responsibility-segregation-cqrs" >}}) often relies on a derived [*OLAP*](https://en.wikipedia.org/wiki/Online_analytical_processing) database to answer queries\. A [*Reporting Database*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) plays the same role for analytical reports\.
  - A [*Memory Image* or *Materialized View*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}) collects state changes from [*event sourcing*](https://martinfowler.com/eaaDev/EventSourcing.html) into an in\-memory state snapshot\.
  - A [*CQRS View Database*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) or [*Query Service*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) collects events from the system’s services to aggregate the data which its clients find useful\.
  - A [*Front Controller*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) keeps track of the status of requests which are being processed by a [*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}})\.
  - An [*External Search Index*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#external-search-index" >}}) supports efficient search in a large collection of documents or data records\.
- An [*Interpreter*]({{< relref "../../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) may compile user scripts which it runs for faster execution\.


## All at once

If your system does not allow for any one of the individual optimizations listed above, there is still a chance to apply them all at once, though not without a certain planning and effort\. In this approach one component injects a part of its business logic, together with preprocessed data to base decisions on, into another component which handles incoming events\. This allows the second component to process certain kinds of incoming messages without any external help or network communication:

<figure>
<a href="/diagrams/Performance/Plugins-injection.png">
<picture>
<source srcset="/diagrams/Performance/Plugins-injection.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Plugins-injection.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Plugins-injection.png" alt="Business logic injection in Layers and Services." loading="lazy" width="1043" height="623" style="width:100%"/>
</picture>
</a>
</figure>

- In [high frequency trading](https://en.wikipedia.org/wiki/High-frequency_trading) it is a common practice to encode simple precomputed trading rules into an FPGA chip or a [programmable network card](https://www.fs.com/blog/fs-smartnic-solutions-understanding-asic-fpga-and-dpu-architectures-26648.html)\. That [removes the delay]({{< relref "../../basic-metapatterns/layers.md#performance" >}}) caused by the host’s operating system and results in nearly instantaneous trading decision and response when the incoming price notification matches one of the preprogrammed trading rules\.
- A service which is originally dependent on other services may instead publish an interface for the other services’ teams to write [*Ambassador Plugins*]({{< relref "../../implementation-metapatterns/plugins.md#ambassador-plugin-logic-extension" >}}) which it would call as [strategies](https://refactoring.guru/design-patterns/strategy) during request processing\. As [plugins]({{< relref "../../implementation-metapatterns/plugins.md" >}}) tend to be co\-located with the host process, the interservice calls disappear, making request processing much faster and more stable\. However, in many cases there will be a need for the service on which behalf the plugin acts to feed an event stream to its plugin to supply the plugin’s decision\-making logic with data, further complicating the practical use of *Ambassador Plugins*\.


## Non\-blocking

When requests access shared resources, they may block or slow each other down\. Therefore the fastest systems either avoid blocking anything during request processing, or avoid sharing resources between their clients:

<figure>
<a href="/diagrams/Variants/2/Data%20Grid.png">
<picture>
<source srcset="/diagrams/Variants/2/Data%20Grid.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Data%20Grid.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Data%20Grid.png" alt="A layer of scaled processing units each connected to a node of an in-memory database over a data replication engine which communicates with a persistent database through readers and writers." loading="lazy" width="1283" height="684" style="width:100%"/>
</picture>
</a>
</figure>

- A component that processes every event with a non\-blocking callback is known as [*Proactor*]({{< relref "../../basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}})\. It grants the component real\-time properties at the cost of hard\-to\-read code\.
- [*Shards*]({{< relref "../../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) dedicate database instances to subsets of the system’s clients, thus lowering the chance for one client’s requests to negatively affect others\.
- [*Actor systems*]({{< relref "../../basic-metapatterns/services.md#actors" >}}) use both of the foregoing latency optimizations: they [dedicate]({{< relref "../../basic-metapatterns/shards.md#temporary-state-create-on-demand-actors" >}}) one non\-blocking *actor* to each client\.
- A [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/sandwich.md#space-based-architecture" >}}) \(shown above\) creates [multiple in\-memory replicas]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) of its data, each serving client requests\. The replicas synchronize their states in background, but there is still a chance of a write conflict when multiple clients simultaneously edit the same data\.


## Miscellaneous optimizations

There are also several optimizations which don’t fit into wider categories:

<figure>
<a href="/diagrams/Variants/2/API%20Composer.png">
<picture>
<source srcset="/diagrams/Variants/2/API%20Composer.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/API%20Composer.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/API%20Composer.png" alt="An API Composer calls services in parallel. A Scatter/Gather or MapReduce calls shards in parallel." loading="lazy" width="1263" height="445" style="width:100%"/>
</picture>
</a>
</figure>

- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) may split a request into subrequests to several services or shards for parallel processing, as shown on the diagram above\.
- [*Request hedging*]({{< relref "../../basic-metapatterns/shards.md#persistent-copy-replica" >}}) is sending an incoming request to multiple copies of the system in parallel and accepting the first response available\. It greatly improves [tail latency](https://en.wikipedia.org/wiki/Tail_latency) and system stability\.
- Fine\-tuning the [*persistence layer*]({{< relref "../../basic-metapatterns/layers.md#data-persistence" >}}) often drastically improves latency\. The techniques range from using a [pair of SQL \+ NoSQL databases]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#specialized-databases" >}}) to [moving the historical data]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#historical-data-data-archiving" >}}) from the main database to a backup storage\.
- [*Shared memory*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-memory" >}}) provides for the fastest interprocess communication\.


## Summary

The common system\-level latency optimization techniques for request processing include bypassing some of the system’s components, co\-locating components with each other or with the client, and preloading or preprocessing the data needed to answer requests\. Though each of these optimizations is powerful on its own, they can be [used together]({{< relref "#all-at-once" >}}) to instantly answer a subset of client requests\. Furthermore, there are specialized optimizations such as request hedging or non\-blocking which improve latency of a single subsystem\.