+++
weight = 3
title = "Choose your own architecture"
+++

# Choose your own architecture

Now that we’ve seen patterns decomposed into decoupling and cohesion, we can try reconstructing architecture based on your project’s needs\.

## Project size

The project’s expected size is among the main determinants of the project’s architecture as both overgrown components and excessive fragmentation handicap development and maintenance\. A moderate number of components of moderate size is the desired zone of comfort\.

Therefore, a one day task will likely be [*monolithic*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}), a man\-month of work needs [*layering*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}) while anything larger calls for at least partial separation into *subdomain* [*modules*]({{< relref "../../part-2--basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) or [*services*]({{< relref "../../part-2--basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}})\. Very large projects may require further subdivision into [*Service\-Oriented Architecture*]({{< relref "../../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) \(*SOA*\) or [*Cell\-Based Architecture*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \(a kind of [*Hierarchy*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md" >}})\)\.

<figure>

<p align="center">
<img src="/Heart/Size-1.png" alt="Size-1" width=100%/>
</p>

</figure>

Any inherent decoupling within your domain is another factor to consider in the initial design\. For example, the layer with domain logic is very likely to contain independent subdomains which naturally make modules or services at next to no development or runtime cost\. Likewise, [*Top\-Down Hierarchy*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) is a good fit for a hierarchical domain\. A domain that builds around stepwise processing of data or events may be modeled as a [*Pipeline*]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}}), which is a very flexible architectural style\.

<figure>

<p align="center">
<img src="/Heart/Size-2.png" alt="Size-2" width=100%/>
</p>

</figure>

The number of teams you start the project with is also important\. For the teams to be as efficient as possible you want them to be almost independent\. As every team gets ownership of one or two components, you must assure that the architecture has enough modules or services for the teams to specialize, because anything shared will likely become a bottleneck\. For example, you can hardly employ more than 3 teams with a [*layered architecture*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}) as there are only so many layers in any system\. Thus, having a large number of teams strongly hints at [*Services*]({{< relref "../../part-2--basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}}), [*SOA*]({{< relref "../../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), or [*Hierarchy*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md" >}})\.

If not all the teams are available from day one, it is still preferable to initially set up component boundaries for the prospective number of teams because subdividing an already implemented component is a terrible experience\. However, it may be [easier and safer](https://martinfowler.com/bliki/MonolithFirst.html) for now to leave all the components running within a single process \(as [*modules*]({{< relref "../../part-2--basic-metapatterns/services.md#asynchronous-modules-modular-monolith-modulith-embedded-actors" >}})\) to avoid the overhead of going distributed and have less trouble moving pieces of code around as needed \(as new requirements often make a joke of your original design\)\. You should be able to make *modules* into [*services*]({{< relref "../../part-2--basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}}) through moderate effort once that becomes an imperative\.

## Domain features

We’ve already seen above that hierarchical or pipelined domains enable the use of corresponding architectures\. There is more to it\.

Sometimes you expect to have many complex use cases which cannot be matched to your subdomains because every scenario involves multiple components, thus spreading over the entire system\. You would usually collect the global use cases into a dedicated component – an [*Orchestrator*]({{< relref "../../part-3--extension-metapatterns/orchestrator.md" >}})\. And if the *Orchestrator* grows out of control, it is [subdivided]({{< relref "../../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers or services\.

<figure>

<p align="center">
<img src="/Heart/Features-1.png" alt="Features-1" width=100%/>
</p>

</figure>

Other systems are built around data\. You cannot split it into private databases because almost every service needs access to the whole which necessitates a [*Shared Repository*]({{< relref "../../part-3--extension-metapatterns/shared-repository.md" >}}), or the highly performant [*Space\-Based Architecture*]({{< relref "../../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.

<figure>

<p align="center">
<img src="/Heart/Features-2.png" alt="Features-2" width=100%/>
</p>

</figure>

Once you go distributed, you will likely employ a [*Middleware*]({{< relref "../../part-3--extension-metapatterns/middleware.md" >}}) to centralize communication between your services\. And you will have various [*Proxies*]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}), such as a [*Firewall*]({{< relref "../../part-3--extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), a [*Reverse Proxy*]({{< relref "../../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and a [*Response Cache*]({{< relref "../../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}})\. You may even deploy a *Proxy* per kind of client if the clients vary in protocols, resulting in [*Backends for Frontends*]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \(*BFF*\)\.

<figure>

<p align="center">
<img src="/Heart/Features-3.png" alt="Features-3" width=100%/>
</p>

</figure>

## Runtime performance

Moreover, there are non\-functional requirements, such as performance and fault tolerance\.

High throughput is achieved by [*sharding*]({{< relref "../../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) or [*replicating*]({{< relref "../../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) your business logic or even your data\. Sharding also helps process huge datasets while replication improves fault tolerance\. [*Space\-Based Architecture*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) replicates the entire dataset in memory for faster access\.

<figure>

<p align="center">
<img src="/Heart/Performance-1.png" alt="Performance-1" width=100%/>
</p>

</figure>

Alternatively, you may use several specialized databases \([*Polyglot Persistence*]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\) or redesign a highly loaded part of your system as a self\-scaling [*Pipeline*]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}})\.

<figure>

<p align="center">
<img src="/Heart/Performance-2.png" alt="Performance-2" width=100%/>
</p>

</figure>

Scalability under uneven load is achieved through [*Function as a Service*]({{< relref "../../part-2--basic-metapatterns/services.md#single-function-faas-nanoservices" >}}) \(*Nanoservices*\), [*Service\-Mesh*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#service-mesh" >}})\-based [*Microservices*]({{< relref "../../part-2--basic-metapatterns/services.md#microservices" >}}) and, to a greater extent, [*Space\-Based Architecture*]({{< relref "../../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\.

<figure>

<p align="center">
<img src="/Heart/Performance-3.png" alt="Performance-3" width=100%/>
</p>

</figure>

Fault tolerance requires you to have [*replicas*]({{< relref "../../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) of every component, including databases, ideally over multiple data centers\. If you are not that rich, be content with [*Actors*]({{< relref "../../part-2--basic-metapatterns/services.md#actors" >}}) or [*Mesh*]({{< relref "../../part-5--implementation-metapatterns/mesh.md" >}})\.

<figure>

<p align="center">
<img src="/Heart/Performance-4.png" alt="Performance-4" width=100%/>
</p>

</figure>

Low latency makes you place simplified first response logic close to your input, leading to:

- [*Model\-View\-Presenter*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}}) or [*Model\-View\-Controller*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) pattern families for user interaction\.
- [*Layers*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}) with [*strategy injection*]({{< relref "../../part-2--basic-metapatterns/layers.md#performance" >}}) for single hardware input\.
- A [*Hierarchy*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md" >}}) for distributed [*control*]({{< relref "../../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) systems such as [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things)\.


<figure>

<p align="center">
<img src="/Heart/Performance-5.png" alt="Performance-5" width=100%/>
</p>

</figure>

## Flexibility

If your product needs customization, you go for [*Plugins*]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}})\.

If it is to survive for a decade, you need [*Hexagonal Architecture*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) to be able to swap vendors\.

If you mediate between resource or service providers and consumers, you build a [*Microkernel*]({{< relref "../../part-5--implementation-metapatterns/microkernel.md" >}})\.

<figure>

<p align="center">
<img src="/Heart/Flexibility-1.png" alt="Flexibility-1" width=100%/>
</p>

</figure>

When your teams develop services and you want them to be [less interdependent]({{< relref "../../part-6--analytics/comparison-of-architectural-patterns/indirection-in-commands-and-queries.md" >}}), you insert an [*Anticorruption Layer*]({{< relref "../../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Open Host Service*]({{< relref "../../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), or [*CQRS View*]({{< relref "../../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) between them\.

<figure>

<p align="center">
<img src="/Heart/Flexibility-2.png" alt="Flexibility-2" width=100%/>
</p>

</figure>

When you have built a large system and really need that thorough data analytics, consider implementing a [*Data Mesh*]({{< relref "../../part-2--basic-metapatterns/pipeline.md#data-mesh" >}})\.

<figure>

<p align="center">
<img src="/Variants/1/Data Mesh.png" alt="Data Mesh" width=100%/>
</p>

</figure>

## Every domain is unique

No one\-size\-fits\-all\. Embedded projects or single\-player games don’t have databases and run in a single process\. High Frequency Trading bypasses the OS kernel to save microseconds\. *Middleware* and distributed databases care about quorum and leader election\. Huge\-scale data processing must account for [bit flips](https://en.wikipedia.org/wiki/Soft_error)\. A medical device should never crash\. Banks store their history forever for external audits\.

There is no universal architecture\. No silver bullet pattern\. Patterns are mere tools\. Know your tools and choose wisely\.

## So it goes

Software architecture lies lifeless in my hands, devoid of its magical colors, *like the dead iguana*\.

<nav>

| \<\< [Deconstructing patterns]({{< relref "../../part-6--analytics/the-heart-of-software-architecture/deconstructing-patterns.md" >}}) | ^ [The heart of software architecture]({{< relref "../../part-6--analytics/the-heart-of-software-architecture/_index.md" >}}) ^ | [Part 7\. Appendices]({{< relref "../../part-7--appendices/_index.md" >}}) \>\> |
| --- | --- | --- |

</nav>



