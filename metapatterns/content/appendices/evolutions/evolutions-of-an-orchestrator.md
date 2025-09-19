+++
weight = 17
title = "Evolutions of an Orchestrator"
description = "An Orchestrator can be subdivided into Backends for Frontends or Layered Services. Alternatively, you can use a layered or hierarchical Orchestrator."
[sitemap]
  priority = 0.3
+++

# Evolutions of an Orchestrator

Employing an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) has two pitfalls:

- The system becomes slower because too much communication is involved\.
- A single *Orchestrator* may be found to be too large and rigid\.


There is one way to counter the first point and more ways to solve the second one:

- Subdivide the *Orchestrator* by the system’s subdomains, forming [*Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md" >}}) and minimizing network communication\.
- Subdivide the *Orchestrator* by the type of client, forming [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.
- Add another [*layer*]({{< relref "../../basic-metapatterns/layers.md" >}}) of orchestration\.
- Build a [*Top\-Down Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


## Subdivide to form Layered Services

<figure>
<a href="/diagrams/Evolutions/2/Orchestrator%20to%20Layered%20Services.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Orchestrator%20to%20Layered%20Services.png" alt="Orchestrator to Layered Services" loading="lazy" width="2529" height="788" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Orchestrated Three\-Layered Services]({{< relref "../../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) \([Layered Services]({{< relref "../../fragmented-metapatterns/layered-services.md" >}}) \([Services]({{< relref "../../basic-metapatterns/services.md" >}}), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\)\)\.

<ins>Goal</ins>: simplify the *Orchestrator*, let the service teams own orchestration, decouple forces for the services, improve performance\.

<ins>Prerequisite</ins>: the high\-level \([orchestration]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}})\) logic is weakly coupled between the subdomains\.

If the *orchestration* logic mostly follows the subdomains, it may be possible to subdivide it accordingly\. Each service gets a part of the *Orchestrator* that mostly deals with its subdomain but may call other services when needed\. As a result, [each service orchestrates every other service]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}})\. Still, a large part of orchestration becomes internal to the service, meaning that fewer calls over the network are involved\.

<ins>Pros</ins>:

- You subdivide the large *Orchestrator* codebase\.
- Performance is improved\.
- The services become more independent in their quality attributes\.


<ins>Cons</ins>:

- You lose the client\-facing *orchestration team* – now each service’s team will need to face its clients\.
- Service teams become interdependent \(while having equal rights\), which may result in slow development and suboptimal decisions\.
- There is no way to share code between different use cases or even take a look at all of the scenarios at once\.


<ins>Further steps</ins>:

- [*CQRS Views*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] or a [*Query Service*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] help a service access and join data that belongs to other services, further reducing the need for interservice communication\.


## Subdivide to form Backends for Frontends

<figure>
<a href="/diagrams/Evolutions/2/Orchestrator%20to%20Backends%20for%20Frontends.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Orchestrator%20to%20Backends%20for%20Frontends.png" alt="Orchestrator to Backends for Frontends" loading="lazy" width="2411" height="752" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Backends for Frontends]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: simplify the *Orchestrator*, employ a team per client type, decouple qualities for clients\.

<ins>Prerequisite</ins>: clients vary in workflows and forces\.

When use cases for clients vary, it makes sense for each kind of client to have a dedicated *Orchestrator*\.

<ins>Pros</ins>: 

- The smaller *Orchestrators* are independent in qualities, technologies and teams\.
- The smaller *Orchestrators* are … well, smaller\.


<ins>Cons</ins>: 

- There is no good way to [share code]({{< relref "../../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}) between the *Orchestrators*\.


<ins>Further steps</ins>:

- You may want to add client\-specific [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}}) and, maybe, co\-locate them with the *Orchestrators* to avoid the extra network hop\.
- Adding another shared [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) below the ones dedicated to clients creates a place for sharing functionality among the *Orchestrators*\.
- If you are running [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}) over a [*Service Mesh*]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}), [*Sidecars*]({{< relref "../../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../../appendices/books-referenced.md#dds" >}})\] may help to share generic code\.


## Add a layer of orchestration

<figure>
<a href="/diagrams/Evolutions/2/Orchestrator%20add%20Orchestrator.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Orchestrator%20add%20Orchestrator.png" alt="Orchestrator add Orchestrator" loading="lazy" width="2451" height="898" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: implement simple use cases quickly, while still supporting complex ones\.

<ins>Prerequisite</ins>: use cases vary in complexity\.

You may use two or three *orchestration frameworks* \(engines\) which differ in complexity\. A simple declarative tool may be enough for the majority of user requests, reverting to custom\-tailored code for rare complex cases\.

<ins>Pros</ins>:

- Simple scenarios are easy to write\.
- You retain good flexibility with hand\-written code when it is needed\.


<ins>Cons</ins>:

- Requires learning multiple technologies\.
- More components mean more failures and more administration\.
- Performance of complex requests may suffer from more indirection\.


<ins>Further steps</ins>:

- Divide one or more of the resulting *orchestration layers* to form [*Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md" >}}), [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), [*Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}), or [*Cell\-Based Architecture*]({{< relref "../../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}})\.


## Form a Hierarchy

<figure>
<a href="/diagrams/Evolutions/2/Orchestrator%20to%20Hierarchy.png" style="outline:none">
<img src="/diagrams/Evolutions/2/Orchestrator%20to%20Hierarchy.png" alt="Orchestrator to Hierarchy" loading="lazy" width="2513" height="604" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Top\-Down Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) \([Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}})\)\.

<ins>Goal</ins>: simplify the *Orchestrator* and, if possible, the services\.

<ins>Prerequisite</ins>: the domain is hierarchical\.

If an *Orchestrator* becomes too complex, some domains \(e\.g\. IIoT or telecom\) encourage using a tree of *Orchestrators*, with each layer taking care of one aspect of the domain, serving the most generic functionality at the root\.

<ins>Pros</ins>: 

- Multiple specialized teams and technologies\.
- Small codebase per team\.
- Reasonable testability\.
- Some decoupling of quality attributes\.


<ins>Cons</ins>:

- Hard to debug\.
- Poor latency in global scenarios unless several layers of the *hierarchy* are colocated\.


<nav>

| \<\< [Evolutions of a Proxy]({{< relref "../../appendices/evolutions/evolutions-of-a-proxy.md" >}}) | ^ [Evolutions]({{< relref "../../appendices/evolutions/_index.md" >}}) ^ | [Evolutions of a Combined Component]({{< relref "../../appendices/evolutions/evolutions-of-a-combined-component.md" >}}) \>\> |
| --- | --- | --- |

</nav>