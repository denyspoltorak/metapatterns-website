+++
weight = 3
title = "Dependency inversion in architectural patterns"
description = "Plugins, Hexagonal Architecture, Microkernel and Hierarchy rely on dependency inversion. Other patterns, such as Layers and Services, occasionally use it."
+++

# Dependency inversion in architectural patterns

I am no fan of [*SOLID*](https://en.wikipedia.org/wiki/SOLID) – to the extent of being unable to remember what those five letters mean – thus I was really surprised to notice that one of its principles, namely [*dependency inversion*](https://en.wikipedia.org/wiki/Dependency_inversion_principle), is quite common with architectural patterns, which means that it is way more generic than *OOP* it is promoted for\.

Let’s see how dependency inversion is used on system level\.

## Patterns that build around it

<figure style="text-align:center">
<a href="/Conclusion/DI-1.png" style="outline:none">
<img src="/Conclusion/DI-1.png" alt="DI-1" style="width:100%"/>
</a>
</figure>

Both [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) and the derived [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) rely on dependency inversion for the same reason – to protect the *core*, which contains the bulk of the code, from variability in the external components it uses\. The *core* operates interfaces \([*SPI*](https://en.wikipedia.org/wiki/Service_provider_interface)s\) which it defines so that it may not care whatever is behind an interface\.

It is the nature of the polymorphic components that distinguishes the patterns:

- [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) allow for small pieces of code, typically contributed by outside developers, to provide customizable parts of the system’s algorithms and decision making\. Oftentimes the core team has no idea of how many diverse plugins will be written for their product\.
- [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) is about breaking dependency of the core on external libraries or services by employing [*adapters*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\. Each adapter depends both on the core’s SPI and on the API of the component which it adapts\. As interfaces and contracts vary among vendors and even versions of software, which we want to be interchangeable, we need adapters to wrap the external components to make them look identical to our core\. Besides, [*stub* or *mock*](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) adapters help develop and test the core in isolation\.


## Patterns that often rely on it

<figure style="text-align:center">
<a href="/Conclusion/DI-2.png" style="outline:none">
<img src="/Conclusion/DI-2.png" alt="DI-2" style="width:100%"/>
</a>
</figure>

A few more metapatterns tend to use this approach to earn its benefits, even though dependency inversion is not among their integral features:

- [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}), yet another metapattern derived from [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}), distributes resources of *providers* among *consumers*\. Polymorphism is crucial for some of its variants, including [*Operating System*]({{< relref "../../implementation-metapatterns/microkernel.md#operating-system" >}}), but may rarely benefit others, such as [*Software Framework*]({{< relref "../../implementation-metapatterns/microkernel.md#software-framework" >}})\.
- [*Top\-Down Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) distributes responsibility over a tree of components\. If the nodes of the tree are polymorphic, they are easier to operate, and there is dependency inversion\. However, in practice, a parent node may often be strongly coupled to the types of its children and access them directly\.
- In another kind of [*Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}), namely [*Cell\-Based Architecture*]({{< relref "../../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \(aka *Services of Services*\), each [*Cell*]({{< relref "../../basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) [may employ](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) a [*Cell Gateway*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) and outbound [*Adapters*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) to isolate its business logic from the environment – just like [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) does for its monolithic core\.


## Patterns that may use it

<figure style="text-align:center">
<a href="/Conclusion/DI-3.png" style="outline:none">
<img src="/Conclusion/DI-3.png" alt="DI-3" style="width:100%"/>
</a>
</figure>

Finally, two basic architectures, [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}), may resort to something similar to dependency inversion to decouple their constituents:

- We often see a higher layer to depend on and a lower layer to implement a standardized interface, like POSIX or SQL, to achieve *interoperability* with other implementations \(which is yet another wording for polymorphism\)\.
- A service may follow the concept of [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) by using an [*Anti\-Corruption Layer*]({{< relref "../../basic-metapatterns/services.md#dependencies" >}}) \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] or [*CQRS Views*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] as [*Adapters*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) that protect it from changes in other system components\.


## Summary

Many architectural patterns employ dependency inversion by adding:

- an *interface* to enable polymorphism of their lower\-level components or 
- [*Adapters*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) to protect a component from changes in its dependencies\.


The two approaches apply in different circumstances:

- If you can enforce your rules of the game on the suppliers of the external components, you merely *define an SPI*, and expect the suppliers to implement and obey it\.
- If the suppliers are independent and it is your side that adapts to their rules, you should *add Adapters* to translate between your lovely SPI and their whimsical APIs\.


<nav>

| \<\< [Pipelines in architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) | ^ [Comparison of architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/_index.md" >}}) ^ | [Indirection in commands and queries]({{< relref "../../analytics/comparison-of-architectural-patterns/indirection-in-commands-and-queries.md" >}}) \>\> |
| --- | --- | --- |

</nav>



