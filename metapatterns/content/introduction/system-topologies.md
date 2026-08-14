+++
weight = 5
title = "System topologies"
description = "This chapter explores common system topologies arranged according to the measure of their partitioning into layers or subdomains."
images = ["/diagrams/Web/og/Topologies.png"]
primary_image = "/diagrams/Topologies/Topologies%20Map.png"
[sitemap]
  priority = 0.5
+++

# System topologies {anchor=false}

In the [previous chapter]({{< relref "../introduction/metapatterns.md" >}}) we started with architectural patterns and grouped them in accordance with their structure and function into *metapatterns*\. Now let’s traverse in the opposite direction: from *topology* \(the structure of a system\) to the patterns which describe it\. We will draw and analyze a map of common system topologies and along the way outline the scope of this book\.

## Methodology

We will rely on our finding that any system has a characteristic representation in the [abstractness\-subdomain\-sharding space]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}})\. The amounts of a system’s partitioning along each of the three dimensions can be used as its coordinates on a map of system topologies:

- *Abstractness* corresponds to the *technical partitioning* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] – subdivision of a system into [*layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with different roles and technologies\. Any use case will likely involve all the layers\.
- *Subdomain* represents the *domain partitioning* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] – segregation of a system into [*modules* or *services*]({{< relref "../basic-metapatterns/services.md" >}}) that encapsulate distinct parts of the business knowledge\. A use case is often localized in one or two subdomains\.
- *Sharding* is about running multiple instances \([*shards*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) or [*replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}})\) of a component\.


<figure>
<a href="/diagrams/Topologies/Partitioning.png">
<picture>
<source srcset="/diagrams/Topologies/Partitioning.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Partitioning.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Partitioning.png" alt="Technical partitioning into Layers, domain partitioning into Services, and multiple instances of a system." loading="lazy" width="1221" height="423" style="width:100%"/>
</picture>
</a>
</figure>

### From theory to practice

Having a distribution of architectures in a 3D space sounds great, but how do we represent it as human\-readable media?

There are at least the following issues:

- Many system topologies feature similar levels of segregation into layers and services, meaning that they belong to the same neighborhood on the map\.
- It makes a difference whether a system’s business logic or its infrastructure is subdivided because business logic comprises the bulk of the code\. Therefore we cannot estimate a system’s coordinates only from the number of layers or services it contains, which means that we are limited to something like “major layering” and “minor layering” instead of numeric values\.
- Sharding of many architectures varies widely and is hard to represent on a flat drawing\.


As a result, the following adjustments were necessary to make the map of system topologies comprehensible:

- Sharding is omitted, transforming 3D coordinates into a flat map\. Yes, much information is lost, but *too much information is no information*\. Now the map is easier to read\.
- I took the liberty of shifting topologies from their real positions to resolve overlaps\.
- Even worse, I moved some of the topologies around to group similar architectures\.
- Exotic \(e\.g\. [*Leaf\-Spine Architecture*]({{< relref "../implementation-metapatterns/mesh.md#leaf-spine-architecture-spine-leaf-architecture" >}})\) and duplicate \(e\.g\. [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}})\) topologies are omitted\.


## The map of system topologies

<figure>
<a href="/diagrams/Topologies/Topologies%20Map.png">
<picture>
<source srcset="/diagrams/Topologies/Topologies%20Map.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Topologies%20Map.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Topologies%20Map.png" alt="A map of system topologies arranged according to the amount of their partitioning into layers and services." loading="lazy" width="2963" height="2083" style="width:100%"/>
</picture>
</a>
</figure>

The map contains only basic architectures which are easy to apprehend and name\. Any complex system is very likely to be a combination of these simple topologies\.

I somewhat arbitrarily divided the map of system topologies into five partially overlapping regions:

- [*Monolithic*]({{< relref "#monolithic-systems" >}}), where the bulk of the system is kept in a single component\.
- [*Layered*]({{< relref "#layered-architectures" >}}), with mostly technical partitioning and specialized components \(drawn in one or two colors which correspond to different kinds of code\)\.
- [*Services*]({{< relref "#services-area" >}}) with domain partitioning, meaning that each of the main components includes several kinds of code\.
- [*Fragmented*]({{< relref "#fragmented-patterns" >}}) *systems* built of many smaller parts\.
- [*Plugins*]({{< relref "#plugins-family" >}}) that usually have a cohesive core and external modular layers\.


This grouping allows us to study the topologies piecemeal without getting lost in their numbers and features\.

## Monolithic systems

In the simplest cases a project is too small for any internal structure to be justified – you can code it in a couple of hours without any preliminary design\. In other cases the domain is known to be so cohesive that you cannot find good module boundaries – any internal interfaces result in much boilerplate code or degrade performance\. Or there is no time left for a thoughtful design\!

### True Monoliths

<figure>
<a href="/diagrams/Topologies/True%20Monoliths.png">
<picture>
<source srcset="/diagrams/Topologies/True%20Monoliths.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/True%20Monoliths.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/True%20Monoliths.png" alt="Diagrams of Monolith, Shards, and Replicas." loading="lazy" width="968" height="503" style="width:100%"/>
</picture>
</a>
</figure>

Few system topologies are truly monolithic with one kind of system components:

- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) keeps everything together in a single cohesive application which makes sense for small, one\-off projects\. A long\-running *Monolith* may need to handle inputs and events, for which there are several options:
  - [*Reactor*]({{< relref "../basic-metapatterns/monolith.md#multi-threaded-reactor-a-thread-per-task" >}}) uses a thread for each request and blocks on calls to the OS or other components\. This is the simplest server\-side implementation\.
  - [*Proactor*]({{< relref "../basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) relies on callbacks that all run in a single thread to achieve real\-time latency and avoid locks\. It is widely used in embedded programming\.
  - [*Half\-Sync/Half\-Async*]({{< relref "../basic-metapatterns/monolith.md#inexact-half-synchalf-async-coroutines-or-fibers" >}}) is an internally layered approach that allocates a coroutine or fiber to each task\. It is more resource\-efficient than [*Reactor*]({{< relref "../basic-metapatterns/monolith.md#multi-threaded-reactor-a-thread-per-task" >}}) but lacks the real\-time responsiveness and flexibility of [*Proactor*]({{< relref "../basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}})\.
- [*Shards*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) are multiple instances of a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), each owning a slice of the system’s data\. A client must know which shard to access either through storing its address or by querying an [*Ambassador Proxy*]({{< relref "../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) library written by the team that deploys the shards\. This is the architecture of choice when clients are independent from each other but the entire dataset is too large to fit in a single server\.
- [*Replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) are instances of a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) with identical data used to achieve fault tolerance and high throughput\. Any writes to one replica must be propagated to the other replicas:
  - With [semi\-specialized replicas]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#read-only-replicas" >}}) all write requests go to a single *leader* instance which publishes the changes for the other replicas, called *followers*, to apply to their datasets\. Read requests usually go to the followers, and the more read traffic there is, the more followers are deployed\.
  - If all the replicas are identical, any of them can handle a write request and publish the update for the other replicas to apply\. This scales write throughput but involves the chance of data conflicts when the same data record is simultaneously changed on multiple replicas\. See [*Data Grid* of *Space\-Based Architecture*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.


### Monoliths with auxiliary layers

<figure>
<a href="/diagrams/Topologies/Monoliths%20with%20Layers.png">
<picture>
<source srcset="/diagrams/Topologies/Monoliths%20with%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Monoliths%20with%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Monoliths%20with%20Layers.png" alt="Diagrams of Monolith with Backends for Frontends, Managed Shards, Peer-to-Peer Mesh, Monolith with a database, and Monolith with Polyglot Persistence." loading="lazy" width="943" height="1003" style="width:100%"/>
</picture>
</a>
</figure>

In other kinds of systems, common in server\-side programming, some functionality moves to a dedicated layer while the business logic remains monolithic:

- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) *with a database* relies on an external [data storage component]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) for persistence\.
- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) *with* [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) uses specialized databases to improve performance\.
- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) *with* [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) employs a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) for each kind of client to address variations in the clients’ protocols and security\.
- *Managed* [*Shards*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) run behind a single [*Sharding Proxy*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) which connects each system’s client to the shard that has that client’s data thus isolating the clients from the knowledge of the system’s internal composition\.
- [*Peer\-to\-Peer Mesh*]({{< relref "../implementation-metapatterns/mesh.md#peer-to-peer-networks" >}}) interconnects multiple instances of an application, acting as a distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}})\.


### Monoliths with Plugins

<figure>
<a href="/diagrams/Topologies/Monoliths%20with%20Plugins.png">
<picture>
<source srcset="/diagrams/Topologies/Monoliths%20with%20Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Monoliths%20with%20Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Monoliths%20with%20Plugins.png" alt="Diagrams of Monolith with Plugins, Model-View-Controller, and Hexagonal Architecture." loading="lazy" width="1103" height="543" style="width:100%"/>
</picture>
</a>
</figure>

A monolithic core can be extended with disposable additions:

- [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) allow for parts of the core’s workflow to be supplied by internal or external teams, customizing the experience of the system’s users without modifications to its main code\.
- [*Model\-View\-Controller* and related patterns]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) provide a [*presentation layer*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) that isolates the main code from dependencies on the UI framework or network protocol, thus minimizing the effort of porting the software to another platform\.
- [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) keeps the entire business logic self\-sufficient by wrapping every dependency with a dedicated [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}), which not only improves portability but also helps with testing and allows for changing vendors late in the development cycle\.


### Underdeveloped Moduliths

<figure>
<a href="/diagrams/Topologies/Underdeveloped%20Moduliths.png">
<picture>
<source srcset="/diagrams/Topologies/Underdeveloped%20Moduliths.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Underdeveloped%20Moduliths.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Underdeveloped%20Moduliths.png" alt="Diagrams of Monolith with libraries and Modulith with shared code." loading="lazy" width="723" height="283" style="width:72%"/>
</picture>
</a>
</figure>

If a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) evolves for a long time, it will likely become segmented into subdomain components, yielding a [*Modulith*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}})\. As that process is not instantaneous, there are a couple of transitional architectures:

- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) *with* [*libraries*]({{< relref "../basic-metapatterns/layers.md#generic-code-libraries-and-utilities" >}}) involves subdomain\-specific third\-party components which are called by its cohesive business logic\.
- [*Modulith*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) *with shared code* has the business logic largely separated into subdomain modules which still rely on a common codebase for shared functionality\.


## Layered architectures

Layering enables the use of specialized technologies and third\-party components while avoiding the [risky](https://martinfowler.com/bliki/MonolithFirst.html) subdivision of business logic\. It also allows for parts of the system to [differ in their qualities, placement, and scalability]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#conflicting-forces" >}})\. All of that makes layered architectures suitable for full\-featured, medium\-sized projects run by one or two teams where both the speed of development and supportability matter\.

### Ordinary Layers

<figure>
<a href="/diagrams/Topologies/Ordinary%20Layers.png">
<picture>
<source srcset="/diagrams/Topologies/Ordinary%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Ordinary%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Ordinary%20Layers.png" alt="Diagrams of DDD-Style Layers, Layers with Polyglot Persistence, Layers with Backends for Frontends, and Monolith with a database." loading="lazy" width="1003" height="443" style="width:100%"/>
</picture>
</a>
</figure>

Typical layered architectures include:

- [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) of various composition, for example:
  - [*Entity\-Control\-Boundary*]({{< relref "../basic-metapatterns/layers.md#entity-control-boundary-ecb-entity-boundary-control-ebc-boundary-control-entity-bce" >}}) which represent the [*domain model*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}), [*use cases*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}), and [*interface*]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}), respectively\. This pattern originated in the age of complex desktop applications\.
  - [*Domain\-Driven Design* decomposition]({{< relref "../basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}) into *presentation* \(interface\), *application* \(use cases\), *domain* \(business rules\), and *infrastructure* \(communication and persistence\)\. It targets enterprise systems\.
  - [*Embedded systems*]({{< relref "../basic-metapatterns/layers.md#embedded-systems" >}}) with pairs of UI \+ HMI, SDK \+ HAL, and FW \+ HW implemented by distinct parties in the supply chain\.
- [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) *with* [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) where the [*persistence*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layer involves multiple databases, usually chosen for their performance with specialized payloads\.
- [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) *with* [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) with a dedicated [*interface*]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) and/or [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) component for each kind of client when the clients differ in their protocols and/or workflows\.
- [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) *with a* [*database*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) as a case of rudimentary layering of server\-side systems\.


### Scaled Layers

<figure>
<a href="/diagrams/Topologies/Scaled%20Layers.png">
<picture>
<source srcset="/diagrams/Topologies/Scaled%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Scaled%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Scaled%20Layers.png" alt="Diagrams of Three-Tier System, MapReduce, Managed Shards, Scaled Service, and Peer-to-Peer Mesh." loading="lazy" width="843" height="883" style="width:87%"/>
</picture>
</a>
</figure>

Several layered architectures build around scalability:

- [*Three\-Tier Architecture*]({{< relref "../basic-metapatterns/layers.md#three-tier-architecture" >}}) contains a frontend layer with an instance per system’s user, scaled backend, and non\-scaled database\. It exploits the physical distribution of the system to reap [cost, performance, and security benefits]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#distribution" >}})\.
- [*Scaled service*]({{< relref "../basic-metapatterns/services.md#scaled-service" >}}) runs multiple instances of a stateless application between a [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), which evenly distributes user requests among the instances, and a [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\. It is the default approach for scaling a server\-side service\.
- [*MapReduce* or *Scatter\-Gather*]({{< relref "../extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) runs a coupled part of a calculation in a non\-scaled layer while mutually independent parts are delegated to multiple worker shards\.
- *Managed* [*Shards*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) rely on a [*Sharding Proxy*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) layer to connect a client to the appropriate shard\. This removes the need for the client to know which shard contains its data\.
- [*Peer\-to\-Peer Mesh*]({{< relref "../implementation-metapatterns/mesh.md#peer-to-peer-networks" >}}) builds a distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) layer that interconnects instances of a client application\.


### Other layered systems

<figure>
<a href="/diagrams/Topologies/Other%20Layered.png">
<picture>
<source srcset="/diagrams/Topologies/Other%20Layered.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Other%20Layered.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Other%20Layered.png" alt="Diagrams of Model-View-Presenter, Onion Architecture, and Sandwich." loading="lazy" width="1003" height="483" style="width:100%"/>
</picture>
</a>
</figure>

Besides that, there are a few peculiar layered systems:

- [*Model\-View\-Presenter* family of patterns]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}}) features layered user interfaces which decouple the main system from a GUI or web framework with the goal of being able to easily switch to another framework version or vendor\.
- [*Onion Architecture* or *Clean Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#ddd-style-hexagonal-architecture-onion-architecture-clean-architecture" >}}) is a [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) \(see [below]({{< relref "#hexagonal-architecture" >}})\) with a layered core structured along the ideas of [*Domain\-Driven Design*]({{< relref "../basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}})\.
- [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}) architectures are [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with the [*domain logic* layer]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) split into subdomains\. It is a pragmatic low effort approach to tackle complexity in quickly evolving projects that can afford several development teams\. It also addresses [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains\.


## Plugins family

Some architectures specialize in separating complex core logic from miscellaneous details to make the *core* independent and reusable under changing conditions\. In most cases the core contains monolithic business logic but that may vary among patterns\. This family of topologies is prevalent in long\-living or highly customizable products whose codebases are too expensive to rewrite to address every trend or fad\.

### Plugin Architecture

<figure>
<a href="/diagrams/Topologies/Plugin%20Architecture.png">
<picture>
<source srcset="/diagrams/Topologies/Plugin%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Plugin%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Plugin%20Architecture.png" alt="A plugin, library, and extension called by a core." loading="lazy" width="723" height="363" style="width:84%"/>
</picture>
</a>
</figure>

[*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) are external components which supply predefined parts of a host component’s workflow\. They may be created by the company that makes the product, often for the sake of selling several flavors with limited or specialized functionality\. Or they may come from external programmers, as codecs in video players or customizations for accounting software, to extend the usefulness of a product without overburdening its core codebase\.

### Separated Presentation

<figure>
<a href="/diagrams/Topologies/Separated%20Presentation.png">
<picture>
<source srcset="/diagrams/Topologies/Separated%20Presentation.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Separated%20Presentation.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Separated%20Presentation.png" alt="Diagrams of Model-View-Presenter and Model-View-Controller." loading="lazy" width="959" height="443" style="width:100%"/>
</picture>
</a>
</figure>

[*Separated Presentation*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#upper-half-separated-presentation-open-host-service" >}}) extracts the [user or network interface]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) functionality into a dedicated layer which is often further subdivided\. This makes the main codebase reusable in different environments:

- The [*Model\-View\-Controller* family of patterns]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) has separate modules for platform\-specific input and for output which is beneficial when there is no web or GUI framework that can provide a unified high\-level user interface\.
- The [*Model\-View\-Presenter* family]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}}) builds on top of a pre\-existing platform\-specific [presentation layer]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}})\. Most of these patterns add an intermediate [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) between the platform\-dependent code and the core application\.


### Control patterns

<figure>
<a href="/diagrams/Topologies/Control%20Patterns.png">
<picture>
<source srcset="/diagrams/Topologies/Control%20Patterns.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Control%20Patterns.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Control%20Patterns.png" alt="Diagrams of Pedestal and Microkernel." loading="lazy" width="883" height="383" style="width:100%"/>
</picture>
</a>
</figure>

A couple of topologies originate with embedded or systems programming where it is important to abstract the business logic from the hardware components which tend to quickly go out of production and thus need to be replaced with incompatible models:

- [*Pedestal*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#pedestal" >}}) wraps each hardware component in a system with a dedicated driver to reduce the dependency of the business logic on hardware specifications thus allowing for the software to be reused with different hardware setups\.
- [*Microkernel Architecture*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) relies on an eponymous layer to mediate between resource consumers and resource producers which implement generic interfaces and thus are replaceable\. This approach is surprisingly ubiquitous:
  - [*Operating systems*]({{< relref "../implementation-metapatterns/microkernel.md#operating-system" >}}) are the origin of [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}), with user space applications competing for system resources owned by the device drivers\.
  - [*Interpreters*]({{< relref "../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) run user scripts in a sandbox and provide them access to installed libraries\.
  - [*Software frameworks*]({{< relref "../implementation-metapatterns/microkernel.md#software-framework-pluggable-component-framework" >}}) follow a similar approach, building a [*Facade*](https://refactoring.guru/design-patterns/facade) to grant user code a managed access to the framework’s internal components\.
  - [*Hypervisors*, *Virtualizers*, and *Distributed Runtimes*]({{< relref "../implementation-metapatterns/microkernel.md#virtualizer-hypervisor-container-orchestrator-distributed-runtime" >}}) abstract a guest operating system or applications from the platform they run on\.


### Hexagonal Architecture

<figure>
<a href="/diagrams/Topologies/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Topologies/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Hexagonal%20Architecture.png" alt="Diagrams of Ports and Adapters and Onion Architecture." loading="lazy" width="923" height="483" style="width:100%"/>
</picture>
</a>
</figure>

A few architectures fully isolate business logic from its environment, resulting in great portability, simpler automated testing and improved separation of concerns:

- [*Ports and Adapters*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#ports-and-adapters-hexagonal-architecture" >}}) \(the original [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})\) inserts an [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) into every communication pathway in or out of its business logic core but does not specify the structure of the core itself, which makes the pattern universally applicable\.
- [*Onion Architecture* or *Clean Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#ddd-style-hexagonal-architecture-onion-architecture-clean-architecture" >}}) structures the core in accordance with the [rules of *Domain\-Driven Design*]({{< relref "../basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}), limiting the applicability of this topology to enterprise systems or complex backends\.


### Cell

<figure>
<a href="/diagrams/Variants/4/Cell.png">
<picture>
<source srcset="/diagrams/Variants/4/Cell.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Cell.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Cell.png" alt="Several intercommunicating subservices are wrapped with a cell gateway that receives client requests, adapters for outgoing communication, and a plugin." loading="lazy" width="1183" height="424" style="width:100%"/>
</picture>
</a>
</figure>

[*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}}) is a building block of huge systems that follow [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}}) or [*Cell\-Based Architecture*]({{< relref "../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services-vertical-slice-architecture-vsa" >}})\. It is a kind of [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) with a modular and often distributed core\. The internals of a *Cell* are hidden behind a [*Cell Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) which implements the *Cell*’s public interface\. Any outgoing communication, initiated from inside the *Cell*, goes through its [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) or through [*Plugins* supplied by peer *Cells*]({{< relref "../implementation-metapatterns/plugins.md#ambassador-plugin-logic-extension" >}})\.

## Services area

Partitioning a system into [*modules*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) or [*services*]({{< relref "../basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}}) which match its subdomains and assigning the components to dedicated teams greatly reduces the cognitive load that the programmers face as each person needs to comprehend only the service they work on\. Given that it is [cognitive load that determines development speed](https://realmensch.org/2018/05/04/we-are-all-10x-developers/), most large projects have service\-based topologies\. 

However, full domain partitioning \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] is [beneficial only when the system’s subdomains are weakly coupled]({{< relref "../foundations-of-software-architecture/modules-and-complexity.md#coupling-and-cohesion" >}}) along every level of their functionality, which is why many real\-world topologies mix cohesive system\-wide layers and decoupled subdomain services\.

### Barebone services

<figure>
<a href="/diagrams/Topologies/Barebone%20Services.png">
<picture>
<source srcset="/diagrams/Topologies/Barebone%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Barebone%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Barebone%20Services.png" alt="Diagrams of Services, Three-Layered Services, Pipeline, and Two-Layered Services." loading="lazy" width="1163" height="663" style="width:100%"/>
</picture>
</a>
</figure>

A few architectures are completely segmented into subdomains:

- Distributed [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) or in\-process [*Modules*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) rely on [*mutual orchestration*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}})\. They come in several kinds:
  - [*Service\-Based Architecture*]({{< relref "../basic-metapatterns/services.md#service-based-architecture-sba-macroservices" >}}) tends to employ single instances of services which cover entire subdomains\. It is used for multi\-team server\-side projects with no special performance considerations\.
  - [*Modulith*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) \(*Modular Monolith*\) runs subdomain\-sized components in a single process, sacrificing fault tolerance for consistency and operational costs\. This architecture fits smaller Internet businesses\.
  - [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) with highly scalable sub\-subdomain components implement high load and high budget systems with well\-established domain knowledge but frequently changing business needs\.
  - [*Actors*]({{< relref "../basic-metapatterns/services.md#actors" >}}) are asynchronous objects used for real\-time tasks that range from embedded telephony to instant messengers to financial systems\. Consider them if you benefit from modeling every user of your system as a lightweight independently acting entity\.
- [*Three\-Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) subdivide each service into the [*use cases*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}), [*domain logic*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}), and [*persistence*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers, allowing for further specialization of staff and technologies\.
- [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) is a [*choreographed*]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) system where each component implements a single stage of data or event processing:
  - [*Pipes and Filters*]({{< relref "../basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}}) is a local and usually linear [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) that processes a data stream\. It is the architecture of choice for systems with customizable workflows and polymorphic algorithms such as video capture or replay\.
  - [*Choreographed Event\-Driven Architecture*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) runs multiple branched [*Pipelines*]({{< relref "../basic-metapatterns/pipeline.md" >}}), each implementing a single use case, over a shared set of services\. It is an easily extendable alternative to [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) for domains with a few highly loaded yet simple scenarios\.
  - [*Data Mesh*]({{< relref "../basic-metapatterns/pipeline.md#data-mesh" >}}) collects, transforms, and processes analytical data from a system of services\.
- [*Two\-Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#choreographed-two-layered-services" >}}) split each component of a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) \(usually a [*Choreographed Event\-Driven Architecture*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}})\) into [*domain logic*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) and [*persistence*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers, emphasizing the use of databases private to their services\. Noticeably, the [*use case logic*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) is present only through the connections between the services\.


### Services with extensions

<figure>
<a href="/diagrams/Topologies/Services%20with%20Extensions.png">
<picture>
<source srcset="/diagrams/Topologies/Services%20with%20Extensions.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Services%20with%20Extensions.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Services%20with%20Extensions.png" alt="Diagrams of Services with a Gateway; Orchestrated Services; Services with: an API Gateway, Backends for Frontends, Shared Repository, Middleware, and Pplyglot Persistence; and of Service Mesh." loading="lazy" width="1132" height="1523" style="width:100%"/>
</picture>
</a>
</figure>

[*Services*]({{< relref "../basic-metapatterns/services.md" >}}) become simpler when common aspects are extracted to a dedicated layer:

- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with a* [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) rely on an external [transport and deployment layer]({{< relref "../basic-metapatterns/layers.md#communication-middleware" >}}) which is usually a framework available off\-the\-shelf:
  - [*Service Mesh*]({{< relref "../extension-metapatterns/middleware.md#service-mesh" >}}) is a distributed [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) for highly scalable systems\.
  - [*Message Bus*]({{< relref "../extension-metapatterns/middleware.md#message-bus" >}}) interconnects services that use different communication technologies by translating between their protocols\. It is useful in integration of legacy systems\.
  - [*Event Mediator*]({{< relref "../extension-metapatterns/middleware.md#event-mediator" >}}) drives communication in [*Event\-Driven Architectures*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}})\.
  - [*Enterprise Service Bus*]({{< relref "../extension-metapatterns/middleware.md#enterprise-service-bus-esb" >}}) is an [*orchestrating*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) that unites several historically separate subsystems into an [*Enterprise Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}})\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with a* [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) share a [data storage or exchange layer]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) and are eligible to implement [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains:
  - [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) simplifies architectural design and makes data synchronization trivial \(see [*Service\-Based Architecture*]({{< relref "../extension-metapatterns/sandwich.md#service-based-architecture" >}})\)\.
  - [*Shared File System*]({{< relref "../extension-metapatterns/shared-repository.md#shared-file-system" >}}) is among the simplest methods of organizing [*Pipelines*]({{< relref "../basic-metapatterns/pipeline.md" >}}) for processing large volumes of data records\.
  - [*Shared Memory*]({{< relref "../extension-metapatterns/shared-repository.md#shared-memory" >}}) is the fastest method of data exchange especially suitable for low latency software\.
  - [*Data Grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is a highly scalable, distributed in\-memory data store of [*Space\-Based Architecture*]({{< relref "../extension-metapatterns/sandwich.md#space-based-architecture" >}})\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with* [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) employ several data stores, usually to improve performance by using each data store in the role it is optimized for\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with a* [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) rely on a shared [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) layer to handle [communication with clients]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}})\. Third\-party *Proxies* reliably cover security and networking concerns with very little effort from the programmers’ side\.
- In [*Orchestrated Services*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) it is the [*use cases*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) which are extracted into a system\-wide layer\. Such subdivision of business logic saves the day when there are many complex system\-wide scenarios while the business rules are specific to particular subdomains\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with an* [*API Gateway*]({{< relref "../extension-metapatterns/proxy.md#api-gateway" >}}) implement public\-API\-related tasks – both [protocol support]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) and [basic orchestration]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) – in a single component which calls underlying services with the [domain logic]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}})\. This is a simplified architecture for ordinary server\-side systems\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with* [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) have a layer of client\-specific components that encapsulate clients’ [protocols]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) and/or [scenarios]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) and are useful when a system serves drastically different kinds of clients\.


### Hierarchies of services

<figure>
<a href="/diagrams/Topologies/Hierarchies%20of%20Services.png">
<picture>
<source srcset="/diagrams/Topologies/Hierarchies%20of%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Hierarchies%20of%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Hierarchies%20of%20Services.png" alt="Diagrams of Cell-Based Architecture and Hierarchical Middleware." loading="lazy" width="1103" height="924" style="width:100%"/>
</picture>
</a>
</figure>

Services are building blocks for a couple of hierarchical architectures used in huge projects:

- [*Cell\-Based Architecture*]({{< relref "../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services-vertical-slice-architecture-vsa" >}}) is a system of clusters of \(often co\-deployed\) services called [*Cells*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\. Recursive decomposition lowers the top\-level system complexity and decouples the subdomains by making their interdependencies explicit\.
- [*Hierarchical Middleware*]({{< relref "../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks-hierarchical-middleware" >}}) interconnects several subsystems of services which belong to different organizations or physical networks\.


### Partially merged services

<figure>
<a href="/diagrams/Topologies/Partially%20Merged%20Services.png">
<picture>
<source srcset="/diagrams/Topologies/Partially%20Merged%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Partially%20Merged%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Partially%20Merged%20Services.png" alt="Diagrams of Sandwich and Modulith with shared code." loading="lazy" width="723" height="283" style="width:73%"/>
</picture>
</a>
</figure>

There are systems in\-between [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) and [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}):

- In [*Modulith*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) *with shared code* the business logic is split into subdomains but still relies on a shared codebase\. It is a transitional architecture often seen in growing projects that [explore subdomain boundaries](https://martinfowler.com/bliki/MonolithFirst.html)\.
- In [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}) only the [*domain logic* layer]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}), which is usually the largest part of the codebase, is segmented into subdomains\. This is the most natural subdivision for many real\-world systems, which inspires multiple architectures:
  - [*Service\-Based Architecture*]({{< relref "../extension-metapatterns/sandwich.md#service-based-architecture" >}}) – the pragmatic approach to server\-side development – often uses a [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) and an [*API Gateway*]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}})\.
  - [*Space\-Based Architecture*]({{< relref "../extension-metapatterns/sandwich.md#space-based-architecture" >}}) provides unparalleled elasticity and scalability for data\-centric domains with its *replicated cache* called [*Data Grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.
  - [*Blackboard Architecture*]({{< relref "../extension-metapatterns/sandwich.md#blackboard-system" >}}) schedules specialized algorithms to solve ill\-structured problems\. 
  - [*Nanoservices*]({{< relref "../extension-metapatterns/sandwich.md#nanoservices" >}}) are independently scalable functions that run in a cloud and share an [*\(API\) Gateway*]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}}) and a [database]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\.


## Fragmented patterns

Finally, some architectures are subdivided into both layers of abstraction and subdomains, resulting in topologies containing many small components\. This happens when interacting parts of a system vary in their qualities and technologies and thus should stay separate, ordinary decomposition results in components too large for comfortable development, or both\.

### Layers of services

<figure>
<a href="/diagrams/Topologies/Layers%20of%20Services.png">
<picture>
<source srcset="/diagrams/Topologies/Layers%20of%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Layers%20of%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Layers%20of%20Services.png" alt="Diagrams of Services with Polyglot Persistence, Services with Backends for Frontends, and Service-Oriented Architecture." loading="lazy" width="1203" height="443" style="width:100%"/>
</picture>
</a>
</figure>

A few topologies are made of layers, each of which is subdivided into services:

- In [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with* [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) there are several specialized data stores with shared access\. This topology may emerge from a [performance optimization]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-shared-repository.md#deploy-specialized-databases" >}}) of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with a* [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\.
- [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) *with* [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) employ a dedicated [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}), or [*API Gateway*]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}}) for each kind of client\. This makes sense when the system's clients have very little in common\.
- [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) features fragmented application, domain, and utility layers, with each component of a higher level calling multiple components from a layer below it\. It enables code reuse, for better or worse, and has reasonably small services even in huge projects but suffers from slow development caused by extensive interdependencies between teams\.


### Layered services

<figure>
<a href="/diagrams/Topologies/Layered%20Services.png">
<picture>
<source srcset="/diagrams/Topologies/Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Topologies/Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Topologies/Layered%20Services.png" alt="Diagrams of Orchestrated Three-Layered Services and Choreographed Two-Layered Services." loading="lazy" width="1123" height="343" style="width:100%"/>
</picture>
</a>
</figure>

More often than not, services are layered internally:

- [*Orchestrated Three\-Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) distinguish between the [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) \(use cases\), [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) \(business rules\), and [*persistence*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) \(database\) layers\.
- [*Choreographed Two\-Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#choreographed-two-layered-services" >}}) contain only the [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) and [*persistence*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers because the [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) logic resides in the graph of connections between the services\.


### Hierarchies

<figure>
<a href="/diagrams/Relations/Hierarchy.png">
<picture>
<source srcset="/diagrams/Relations/Hierarchy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Hierarchy.png" alt="Diagrams of Orchestrator of Orchestrators, Middleware of Middlewares, and Services of Services." loading="lazy" width="1463" height="426" style="width:100%"/>
</picture>
</a>
</figure>

Finally, there are [hierarchical]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) topologies with recursive partitioning:

- [*Top\-Down Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) is arguably the best way to implement a system that involves many kinds of somewhat related entities\. It emerges in domains as diverse as compilers, industrial automation, graphical user interfaces, and online marketplaces\.
- [*Hierarchical Middleware*]({{< relref "../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks-hierarchical-middleware" >}}) interconnects subsystems that differ in their communication protocols\.
- [*Cell\-Based Architecture*]({{< relref "../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services-vertical-slice-architecture-vsa" >}}) splits every large subdomain service into a group of subservices encapsulated with a [*Cell Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}})\. This keeps individual services small without spreading hundreds of them into the system level\.


## Common motifs

Every area of the topologies map highlights certain design principles:

- Small and simple systems may stay cohesive as [*Monoliths*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}})\.
- Medium\-sized software benefits from functional partitioning \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] into [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Long\-lived projects become stabilized by extracting any volatile code into expendable modules\. Different applications of this principle yield [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}), [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}), and [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\.
- Large software is decomposed into subdomains owned by dedicated teams\. See [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) and [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.
- Huge systems require recursive decomposition as found in [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) and [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\.


Other motifs are harder to notice as they apply to both scaled layered systems and those subdivided into services:

- There is often a *managing layer* that makes use of underlying components:
  - A [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) is an [*interface*]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) that receives and pre\-processes client input, then forwards the resulting request to whatever is behind it\.
  - An [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) is an [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) that implements complex use cases which turn a single event or client request into a chain of calls to the lower layer\.
  - [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) segment a managing layer into client\-specific services\.
- A *platform layer* provides some functionality to other system components:
  - A [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) deploys and [interconnects]({{< relref "../basic-metapatterns/layers.md#communication-middleware" >}}) [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) or [*Replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}})\.
  - A [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) stores the system’s [data]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}), offering consistency and persistence\.
  - [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) subdivides a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) layer\.
- [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}) wraps [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) or [*Replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) with both managing and platform layers\.
- A [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) interconnects any components that use it\.


As we see, [metapatterns]({{< relref "../introduction/metapatterns.md#structure-determines-architecture" >}}) emerge as archetypes shared among system topologies\.

## Summary

There are many system topologies with various degrees of segregation into layers and subdomains\. No single architecture is a silver bullet, each topology has its use depending on the circumstances\. The following chapters of this book explore archetypes shared among topologies which are called *metapatterns*\.