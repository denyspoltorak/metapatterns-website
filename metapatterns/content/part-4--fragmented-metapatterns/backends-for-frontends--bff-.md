+++
weight = 8
title = "Backends for Frontends (BFF)"
+++

# Backends for Frontends \(BFF\)

<figure style="text-align:center">
<a href="/Main/Backends%20for%20Frontends.png" style="outline:none">
<img src="/Main/Backends%20for%20Frontends.png" alt="Backends for Frontends" width=100%/>
</a>
</figure>

*Hire a local guide\.* Dedicate a service for every kind of client\.

<ins>Known as:</ins> Backends for Frontends \(BFF\), [Layered Microservice Architecture](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md)\.

<ins>Aspects:</ins>

- [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}),
- [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.


<ins>Variants:</ins> 

Applicable to:

- Proxies, 
- Orchestrators,
- Proxy \+ Orchestrator pairs,
- API Gateways,
- Event Mediators\.


<ins>Structure:</ins> A layer of integration services over a shared layer of core services\.

<ins>Type:</ins> Extension, derived from [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) and/or [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Clients become independent in protocols, workflows, and, to an extent, forces | No single place for cross\-cutting concerns |
| A specialized team and technology per client may be employed | More work for the DevOps team |
| The multiple *Orchestrators* are smaller and more cohesive than the original universal one |  |


<ins>References:</ins> The [original article](https://samnewman.io/patterns/architectural/bff/), a [smaller one](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends) from Microsoft and an [excerpt](https://microservices.io/patterns/apigateway.html) from \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\. Here are [reference diagrams](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md) from WSO2 \(notice multiple *Microgateway* \+ *Integration Microservice* pairs\)\.

If some aspect\(s\) of serving our system’s clients strongly vary by client type \(e\.g\. OLAP vs OLTP, user vs admin, buyer vs seller vs customer support\), it makes sense to use a dedicated component \(the titular *Backend for Frontend* or *BFF*\) per client type to encapsulate the variation\. Protocol variations call for multiple [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), workflow variations – for several [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), both coming together – for [*API Gateways*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) or *Proxy \+ Orchestrator* pairs\. It is even possible to vary the *BFF*’s programming language on a per client basis\. The drawback is that once the clients get their dedicated *BFFs* it becomes hard to share a common functionality between them, unless you are willing to add yet another new utility [*service*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or [*layer*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) that can be used by each of them \(and that will strongly smell of [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\)\.

### Performance

As the multiple *Orchestrators* of *BFF* don’t intercommunicate, the pattern’s performance is identical to that of an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}): it also slows down request processing in the general case but allows for several [specific optimizations]({{< relref "../part-3--extension-metapatterns/orchestrator.md#performance" >}}), including direct communication channels between orchestrated [services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

### Dependencies

Each *BFF* depends on all the services it uses \(usually every service in the system\)\. The services themselves are likely to be independent, as is common in [*orchestrated* systems]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}})\.

<figure style="text-align:center">
<a href="/Dependencies/Backends%20for%20Frontends.png" style="outline:none">
<img src="/Dependencies/Backends%20for%20Frontends.png" alt="Backends for Frontends" width=91%/>
</a>
</figure>

### Applicability

*Backends for Frontends* are <ins>good</ins> for:

- *Multiple client protocols\.* Deploying a [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per protocol hides the variation from the underlying system\.
- *Multiple UIs\.* When you have one team per UI, each of them may [want to have](https://netflixtechblog.com/embracing-the-differences-inside-the-netflix-api-redesign-15fd8b3dc49d) an API which they feel comfortable with\.
- *Drastically different workflows\.* Let each client\-facing development team own a component and choose the best fitting technologies and practices\.


*Backends for Frontends* <ins>should be avoided</ins> when:

- *The clients are mostly similar\.* It is hard to share code and functionality between *BFF*s\. If the clients have much in common, the shared aspects either find their place in a shared monolithic layer \(e\.g\. multiple client protocols call for multiple [*Gateways*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) but a shared [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) or are duplicated\. *BFF* may not be the best choice – use OOD \(conditions, factories, strategies, inheritance\) instead to handle the clients’ differences within a single codebase\.


### Relations

<figure style="text-align:center">
<a href="/Relations/BFF.png" style="outline:none">
<img src="/Relations/BFF.png" alt="BFF" width=100%/>
</a>
</figure>

*Backends for Frontends*:

- Extends [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or rarely [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), or [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\.
- Is derived from a client\-facing extension metapattern: [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}), or [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}})\.


## Variants

*Backends for Frontends* vary in the kind of component that gets dedicated to each client:

### Proxies

<figure style="text-align:center">
<a href="/Variants/3/BFF%20-%20Gateways.png" style="outline:none">
<img src="/Variants/3/BFF%20-%20Gateways.png" alt="BFF - Gateways" width=100%/>
</a>
</figure>

Dedicating a [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per client is useful when the clients differ in the mode of access to the system \(protocols / encryption / authorization\) but not in workflows\.

Multiple *Adapters* match the literal meaning of *Backends for Frontends* – each UI team \([backend, mobile, desktop](https://www.thoughtworks.com/insights/blog/bff-soundcloud); or [end\-device\-specific](https://netflixtechblog.com/embracing-the-differences-inside-the-netflix-api-redesign-15fd8b3dc49d) teams\) gets some code on the backend side to adapt the system’s API to its needs by building a new, probably more high\-level specialized API on top of it\.

### Orchestrators

<figure style="text-align:center">
<a href="/Variants/3/BFF%20-%20Orchestrators.png" style="outline:none">
<img src="/Variants/3/BFF%20-%20Orchestrators.png" alt="BFF - Orchestrators" width=100%/>
</a>
</figure>

An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) per client makes sense if the clients use the system in completely unrelated ways, e\.g\. a shop’s customers have little to share with its administrators\.

### Proxy \+ Orchestrator pairs

<figure style="text-align:center">
<a href="/Variants/3/BFF%20-%20Gateways%20+%20Orchestrators.png" style="outline:none">
<img src="/Variants/3/BFF%20-%20Gateways%20+%20Orchestrators.png" alt="BFF - Gateways + Orchestrators" width=100%/>
</a>
</figure>

Clients vary in both access mode \(protocol\) and workflow\. [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) or [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) may be reused if some kinds of clients share only protocol or application logic\.

### API Gateways

<figure style="text-align:center">
<a href="/Variants/3/BFF%20-%20API%20gateways.png" style="outline:none">
<img src="/Variants/3/BFF%20-%20API%20gateways.png" alt="BFF - API gateways" width=100%/>
</a>
</figure>

Clients vary in access mode \(protocol\) and workflow and there is a third\-party [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) framework which seems to fit your requirements off the shelf\.

### Event Mediators

<figure style="text-align:center">
<a href="/Variants/3/BFF%20-%20Event%20mediators.png" style="outline:none">
<img src="/Variants/3/BFF%20-%20Event%20mediators.png" alt="BFF - Event mediators" width=100%/>
</a>
</figure>

\[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] mentions that multiple [*Event Mediators*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}) may be deployed in [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) to split the codebase and improve stability\.

## Evolutions

*BFF*\-specific evolutions aim at sharing logic between the *BFF*s:

- The *BFF*s can be merged into a single [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) if their functionality becomes mostly identical\.
- A shared *orchestration* [*layer*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) with common functionality may be added for use by the *BFF*s\.
- A layer of *Integration Services* under the *BFF*s simplifies them by providing shared high\-level APIs for the resulting [*Cells*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.
- [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] \(of [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}})\) are a way to share libraries among the *BFF*s\.


<figure style="text-align:center">
<a href="/Evolutions/3/BFF.png" style="outline:none">
<img src="/Evolutions/3/BFF.png" alt="BFF" width=100%/>
</a>
</figure>

## Summary

*Backends for Frontends* assigns a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) and/or an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) per each kind of a system’s client to encapsulate client\-specific use cases and protocols\. The drawback is that there is no decent way for sharing functionality between the *BFF*s\.

<nav>

| \<\< [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Service\-Oriented Architecture \(SOA\)]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) \>\> |
| --- | --- | --- |

</nav>



