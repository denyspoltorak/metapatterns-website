+++
weight = 10
title = "Hierarchy"
description = A Hierarchy distributes responsibilities throughout a tree of components. It is fault tolerant, and the components remain simple and are easy to replace.
+++

# Hierarchy

<figure style="text-align:center">
<a href="/Main/Hierarchy.png" style="outline:none">
<img src="/Main/Hierarchy.png" alt="Hierarchy" style="width:100%"/>
</a>
</figure>

*Command and conquer\.* Build a tree of responsibilities\.

<ins>Variants:</ins>

By structure:

- Polymorphic children,
- Functionally distinct children\.


By direction:

- Top\-Down Hierarchy / Orchestrator of Orchestrators / [Presentation\-Abstraction\-Control](https://en.wikipedia.org/wiki/Presentation%E2%80%93abstraction%E2%80%93control) \(PAC\) \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}}), [POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\] / [Hierarchical Model\-View\-Controller](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#hierarchical-model-view-controller) \(HMVC\),
- Bottom\-Up Hierarchy / Bus of Buses / Network of Networks,
- In\-Depth Hierarchy / [Cell\-Based \(Microservice\) Architecture](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) \(WSO2 version\) / [Segmented Microservice Architecture](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md) / Services of Services / Clusters of Services \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\.


<ins>Structure:</ins> A tree of components\.

<ins>Type:</ins> Main or extension\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Very good in decoupling logic | Global use cases may be hard to debug |
| Supports multiple development teams and technologies | Poor latency for global use cases |
| Components may vary in qualities | Operational complexity |
| Low\-level components are easy to replace | Slow start of the project |


<ins>References:</ins> None good I know of\.

Though not applicable to every domain, hierarchical decomposition is arguably the best way to distribute responsibilities between components\. It limits the connections \(thus the number of interfaces and contracts to keep in mind\) of each component to its parent and a few children, allowing for the building of complex \(and even complicated\) systems in a simple way\. The hierarchical structure is very flexible as it features [multiple layers of indirection](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering) \(and often polymorphism\), which makes addition, replacement, or [stubbing/mocking](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) of leaf components trivial\. It is also quite fault\-tolerant as individual subtrees operate independently\. 

This architecture is not ubiquitous because few domains are truly hierarchical\. Its high fragmentation results in increased latency and poor debugging experience\. Moreover, component interfaces should be designed beforehand and are hard to change\.

### Performance

No kind of distributed hierarchy is latency\-friendly as many use cases involve several network hops\. The fewer layers of the hierarchy are involved in a task, the better its performance\.

<figure style="text-align:center">
<a href="/Performance/Hierarchy%20-%20speed.png" style="outline:none">
<img src="/Performance/Hierarchy%20-%20speed.png" alt="Hierarchy - speed" style="width:100%"/>
</a>
</figure>

Maintaining high throughput usually requires deploying multiple instances of the root component, which is not possible if it is stateful \(in [*control systems*]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}})\) and the state cannot be split into [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}})\. The following tricks may help unloading the root:

- *Aggregation* \(first met in [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\): a node of a hierarchy collects reports from its children, aggregates them into a single package, and sends the aggregated data up to its parent\. This greatly reduces traffic to the root in large [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things) networks\.
- *Delegation* \(resembles strategy injection and batching for [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\): a node should try to handle all the low\-level details of communication with its children without consulting its parent node\. For a [control system]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) that means that its mid\-level nodes should implement control loops for the majority of incoming events\. For a [processing system]({{< relref "../part-1--foundations/four-kinds-of-software.md#computational-single-run-user-input" >}}) that means that its mid\-level nodes should expose coarse\-grained interfaces to their parent\(s\) while translating each API method call into multiple calls to their child nodes\.
- *Direct communication channels* \(previously described for [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\): if low\-level nodes need to exchange data, their communication should not always go through the higher\-level nodes\. Instead, they may negotiate a direct link \(open a socket\) that bypasses the root of the hierarchy\.


<figure style="text-align:center">
<a href="/Performance/Hierarchy%20-%20optimizations.png" style="outline:none">
<img src="/Performance/Hierarchy%20-%20optimizations.png" alt="Hierarchy - optimizations" style="width:100%"/>
</a>
</figure>

### Dependencies

A parent node would usually define one \(for polymorphic children\) or more \(otherwise\) [*SPIs*](https://en.wikipedia.org/wiki/Service_provider_interface) for its child nodes to implement\. The interfaces reside on the parent side because low\-level nodes tend to be less stable \(new types of them are often added and old ones replaced\) therefore we don’t want our main business logic to depend on them\.

<figure style="text-align:center">
<a href="/Dependencies/Hierarchy.png" style="outline:none">
<img src="/Dependencies/Hierarchy.png" alt="Hierarchy" style="width:100%"/>
</a>
</figure>

### Applicability

*Hierarchy* <ins>fits</ins> with:

- *Large and huge projects\.* The natural division by both level of abstractness and subdomain allows for using smaller modules, ideally with intuitive interfaces\. The APIs for each team to learn are limited to just a few which their component interacts with directly\.
- *Systems of hardware devices\.* Real\-world [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things) systems may use a hierarchy of controllers to benefit from autonomous decision\-making and data aggregation\.
- *Customization*\. The tree\-like structure provides opportunities for easy customization\. A medium\-sized hierarchical system may integrate hundreds of leaf types\.
- *Survivability*\. A distributed hierarchy retains limited functionality even if several of its nodes fail\.


*Hierarchy* <ins>fails</ins> with:

- *Cohesive domains\.* Horizontal interactions \(those between nodes that belong to the same layer\) bloat interfaces as they have to pass through parent nodes\.
- *Quick start*\. Finding \(and verifying\) a good hierarchical domain model may be hard if at all possible\. Debugging an initial implementation will not be easy\.
- *Low latency*\. System\-wide scenarios involve many cross\-component interactions which are slow in distributed systems\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Hierarchy.png" style="outline:none">
<img src="/Relations/Hierarchy.png" alt="Hierarchy" style="width:100%"/>
</a>
</figure>

*Hierarchy*:

- Can be applied to [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})*,* [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) or [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.


## Variants by structure \(may vary per node\)

*Hierarchy* comes in various shapes as it is more of a design approach than a ready\-to\-use pattern:

### Polymorphic children

All the managed child nodes expose the same interface and contract\. This tends to simplify the implementation of the parent node and resembles inheritance of OOD\.

Example: a fire alarm system may treat all of its fire sensors as identical devices, even though the real hardware comes from many manufacturers\.

### Functionally distinct children

The managing node is aware of several kinds of children that vary in their APIs and contracts, just like with composition in OOD\.

Example: an intrusion alarm logic may need to discern between cat\-affected IR sensors and mostly cat\-proof glass break detectors\.

## Variants by direction

### Top\-Down Hierarchy, Orchestrator of Orchestrators, Presentation\-Abstraction\-Control \(PAC\), Hierarchical Model\-View\-Controller \(HMVC\)

<figure style="text-align:center">
<a href="/Variants/3/Hierarchy%20-%20Top-down.png" style="outline:none">
<img src="/Variants/3/Hierarchy%20-%20Top-down.png" alt="Hierarchy - Top-down" style="width:100%"/>
</a>
</figure>

In the most common case *Hierarchy* is applied to business logic to build a layered system which grows from a single generic high\-level root into a swarm of specialized low\-level pieces\. The most obvious applications are protocol parsers, decision trees, [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things) \(e\.g\. a fire alarm system of a building\), and [modern automotive](https://semiengineering.com/managing-todays-advanced-vehicle-networks-design-challenges/) networks\. A marketplace that allows for customized search and marketing algorithms within each category of its goods may also be powered by a hierarchy of category\-specific services\.

[*Presentation\-Abstraction\-Control*](https://en.wikipedia.org/wiki/Presentation%E2%80%93abstraction%E2%80%93control) \(*PAC*\) \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}}), [POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\] applies *Top\-Down Hierarchy* to a user\-facing application, providing each of the resulting [*layered*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) nodes with its own widget \(*presentation*\) on the UI screen \(which is the *presentation* of the root node\)\. *Controls* are responsible for inter\-node communication and integration logic, while domain logic and data reside in *abstractions*\.

[*Hierarchical Model\-View\-Controller*](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#hierarchical-model-view-controller) \(*HMVC*\) is similar, but its views access models directly, like in [*MVC*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}), and every model synchronizes with the global data\. This pattern [was used](https://web.archive.org/web/20060319064042/http://www.javaworld.com/javaworld/jw-09-2000/jw-0908-letters.html) in [rich clients](https://en.wikipedia.org/wiki/Rich_client)\.

<figure style="text-align:center">
<a href="/Variants/3/PAC.png" style="outline:none">
<img src="/Variants/3/PAC.png" alt="PAC" style="width:100%"/>
</a>
</figure>

### Bottom\-Up Hierarchy, Bus of Buses, Network of Networks

<figure style="text-align:center">
<a href="/Variants/3/Hierarchy%20-%20Bottom-up.png" style="outline:none">
<img src="/Variants/3/Hierarchy%20-%20Bottom-up.png" alt="Hierarchy - Bottom-up" style="width:100%"/>
</a>
</figure>

Other cases require building a common base for intercommunication between several networks which vary in their protocols \(and maybe even their hardware\)\. The root of such a *Hierarchy* is a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) generic and powerful enough to cover the needs of all the specialized networks which it interconnects\.

Example: [Automotive networks](https://www.mdpi.com/1424-8220/21/23/7917), integration of corporate networks, [the Internet](https://en.wikipedia.org/wiki/Internet_service_provider)\.

### In\-Depth Hierarchy, Cell\-Based \(Microservice\) Architecture \(WSO2 version\), Segmented Microservice Architecture, Services of Services, Clusters of Services

<figure style="text-align:center">
<a href="/Variants/3/Cell-Based%20Architecture.png" style="outline:none">
<img src="/Variants/3/Cell-Based%20Architecture.png" alt="Cell-Based Architecture" style="width:100%"/>
</a>
</figure>

When several [*services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) in a system grow large, in some cases it is possible to divide each of them into *subservices*\. Each group of the resulting subservices \(known as a [*Cell*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}), [*Domain*](https://www.uber.com/blog/microservice-architecture/) or *Cluster* \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\) usually implements a *bounded context* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\. It is hidden behind its own [*Cell Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) and may even use its own [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. Subservices of a *Cell* may [*share a database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) and may be deployed as a single unit\. This keeps the system’s integration complexity \(the length of its APIs and the number of deployable units\) reasonable while still scaling development among many teams, each owning a service\. If each instance of a *Cell* owns a [*shard*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) of its database, the system [becomes more stable](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/what-is-a-cell-based-architecture.html) as there is no single point of failure \(except for the [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) called *Cell Router*\)\. Another benefit is that *Cells* can be deployed to regional data centers to improve locality for users of the system\. However, that will likely cause data synchronization traffic between the data centers\.

The [*Cell\-Based Architecture*](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) \([*Segmented Microservice Architecture*](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md)\) may be seen as a combination of an *Orchestrator of Orchestrators* and a *Bus of Buses* where the subservices are leaf nodes of both *hierarchies* while the [*API Gateways*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) of the *Cells* are their internal nodes\.

Uber [compacted](https://www.uber.com/blog/microservice-architecture/) 2200 [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) into 70 *Cells* arranged in [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\-style [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) called [*Domain\-Oriented Microservice Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}})\.

## Evolutions

- The upper component of a *Top\-Down Hierarchy* can be split into [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/3/Hierarchy%20-%201.png" style="outline:none">
<img src="/Evolutions/3/Hierarchy%20-%201.png" alt="Hierarchy - 1" style="width:100%"/>
</a>
</figure>

## Summary

*Hierarchy* fits a project of any size as it evenly distributes complexity among the system’s many components\. However, it is not without drawbacks in performance, debuggability, and operational complexity\. Moreover, very few domains allow for seamless application of this architecture\.

<nav>

| \<\< [Service\-Oriented Architecture \(SOA\)]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) | ^ [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) ^ | [Part 5\. Implementation Metapatterns]({{< relref "../part-5--implementation-metapatterns/_index.md" >}}) \>\> |
| --- | --- | --- |

</nav>



