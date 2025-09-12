+++
weight = 9
title = "Orchestrator"
description = An Orchestrator integrates lower-level components. It runs a client request as a series of calls to other components while keeping their states consistent.
+++

# Orchestrator

<figure style="text-align:center">
<a href="/Main/Orchestrator.png" style="outline:none">
<img src="/Main/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

*One ring to rule them all\.* Make a service to integrate other services\.

<ins>Known as:</ins> Orchestrator \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], Orchestrated Services, Service Layer \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\], Application Layer \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\], Wrapper Facade \[[POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\], Multi\-Worker \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\], Controller / Control, Workflow Owner \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] of [Microservices]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}), and Processing Grid \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] of [Space\-Based Architecture]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}})\.

<ins>Aspects:</ins>

- Mediator \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}}), [SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\],
- Facade \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\]\.


<ins>Variants:</ins> 

By transparency:

- Closed or strict,
- Open or relaxed\.


By structure \(not exclusive\):

- Monolithic,
- Sharded, 
- Layered \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\],
- A service per client type \([*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\),
- A service per subdomain \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] \([*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\),
- A service per use case \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] \([*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\-style\)\.


By function:

- API Composer \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / Remote Facade \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] / [Gateway Aggregation](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation) / Composed Message Processor \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] / [Scatter\-Gather](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}}), [DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] / [MapReduce](https://en.wikipedia.org/wiki/MapReduce) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\],
- Process Manager \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] / Orchestrator \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], 
- \(Orchestrated\) Saga \[[LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] / Saga Orchestrator \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / [Saga Execution Component](https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf) / Transaction Script \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] / Coordinator \[[POSA3]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa3" >}})\],
- [Integration \(Micro\-\)Service](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md) / Application Service,
- \(with a [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\) API Gateway \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] / Microgateway, 
- \(with a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\) Event Mediator \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], 
- \(with a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and [*Adapters*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\) Enterprise Service Bus \(ESB\) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]\.


<ins>Structure:</ins> A layer of high\-level business logic built on top of lower\-level services\.

<ins>Type:</ins> Extension\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Separates integration concerns from the services – decouples the services’ APIs | May increase latency for global use cases |
| Global use cases can be changed and deployed independently from the services | Qualities of the services become coupled to an extent |
| Decouples the services from the system’s clients | API design is an extra step before implementation |


<ins>References:</ins> \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] discusses orchestration in its chapters on *Event\-Driven Architecture*, *Service\-Oriented Architecture*, and *Microservices*\. \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] describes orchestration\-based *Sagas* and its Order Service acts as an *Application Service* without explicitly naming the pattern\. \[[POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}})\] defines several variants of *Facade*\.

An *Orchestrator* takes care of global use cases \(those involving multiple services\) thus allowing each service to specialize in its own subdomain and, ideally, forget about the existence of all the other services\. This way the entire system’s high\-level logic \(which is subject to frequent changes\) is kept \(and deployed\) together, isolated from usually more complex subdomain\-specific services\. Dedicating a [layer]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) to global scenarios makes them relatively easy to implement and debug, while the corresponding development team that communicates with clients shelters other narrow\-focused teams from disruptions\. The cost of employing an *Orchestrator* is both degraded performance when compared to basic [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) that rely on [*choreography*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}}), [MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] and some coupling of the properties of the orchestrated services as the *Orchestrator* usually treats every service in the same way\.

An *Orchestrator* fulfills two closely related roles:

- As a *Mediator* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}}), [SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] it keeps the states of the underlying components \(services\) consistent by propagating changes that originate in one component to the rest of the system\. This role is prominent in [*control* software]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}), pervading automotive, aerospace, and IoT industries\. The *Mediator* role also emerges as [*Saga*]({{< relref "#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\.
- As a *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] it builds high\-level scenarios out of smaller steps provided by the services or modules it controls\. This role is obvious for [*processing* systems]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}}) where clients communicate with the *Facade*, but it is also featured in *control* software, because sometimes a simple event may trigger a complex multi\-component scenario managed by the system’s *Orchestrator*\.


<figure style="text-align:center">
<a href="/Misc/Orchestrator.png" style="outline:none">
<img src="/Misc/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

[Data *processing*]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}}) systems, such as backends, may deploy multiple [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of stateless *Orchestrators* to improve stability and performance\. In contrast, an *Orchestrator* in [*control* software]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) incorporates the highest\-level view of the system’s state thus it cannot be easily replicated \(as any replicated state must be kept synchronized, introducing delay or inconsistency in decision\-making\)\.

### Performance

When compared to [*choreography*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}), [*orchestration*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) usually worsens latency as it involves extra steps of communication between the *Orchestrator* and orchestrated components\. However, the effects should be estimated on case by case basis, as there are exceptions in at least the following cases:

- An *Orchestrator* may cache the state of the orchestrated system, gaining the ability to immediately respond to read requests with no need to query the underlying components\. This is very common with [*control* systems]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\.
- An *Orchestrator* may persist a write request, respond to the client, and then start the actual processing\. Persistence grants that the request will eventually be completed as it can be restarted\.
- An *Orchestrator* may run multiple subrequests in parallel, reducing latency compared to a chain of choreographed events\.
- In a highly loaded or latency\-critical system, orchestrated services may establish direct data streams that bypass the *Orchestrator*\. A classic example is [VoIP](https://en.wikipedia.org/wiki/Voice_over_IP) where the call establishment logic \(SIP\) goes through an orchestrating server while the voice or video \(RTP\) is streaming directly between the clients\.


<figure style="text-align:center">
<a href="/Performance/Orchestrator.png" style="outline:none">
<img src="/Performance/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

I don’t see how orchestration can affect throughput as in most cases the *Orchestrator* can be scaled\. However, scaling weakens consistency as then no instance of the *Orchestrator* has exclusive control over the system’s state\.

### Dependencies

An *Orchestrator* may depend on the *APIs* of the services it orchestrates or define *SPIs* for them to implement, with the first mode being natural for its *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] aspect and the second one for the *Mediator* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\]:

<figure style="text-align:center">
<a href="/Dependencies/Orchestrator.png" style="outline:none">
<img src="/Dependencies/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

If an *Orchestrator* is added to integrate existing components, it will use their APIs\.

In large projects, where each service gets a separate team, the APIs need to be negotiated beforehand, and will likely be owned by the orchestrated services\.

Smaller \(single\-team\) systems tend to be developed top\-down, with the *Orchestrator* being the first component to implement, thus it defines the interfaces it uses\. 

Likewise, [control systems]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) tend to reverse the dependencies, with their services depending on the orchestrator’s SPI – either because their events originate with the services \(so the services must have an easy way to contact the *Orchestrator*\) or to provide for polymorphism between the low\-level components\. See the [chapter on *orchestration*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) for more details\.

### Applicability

*Orchestrators* <ins>shine</ins> with:

- *Large projects\.* The partition of business logic into a high\-level application \(*Orchestrator*\) and the multiple [subdomain *Services*]({{< relref "../part-2--basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}}) it relies on provides perfect code decoupling and team specialization\.
- *Specialized teams\.* As an improvement over [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), the teams which develop deep knowledge of subdomains will delegate communication with customers to the application team\.
- *Complex and unstable requirements*\. The *integration* layer \(*Orchestrator*\) should be high\-level and simple enough to be easily extended or modified to cover most of the customer requests or marketing experiments without much help from the domain teams\.


*Orchestrators* <ins>fail</ins> in:

- *Huge projects\.* At least one aspect of complexity is going to hurt\. Either the number of the subdomain services and the size of their APIs will make it impossible for an *Orchestrator* programmer to find the correct methods to call, or the *Orchestrator* itself will become unmanageable due to the number and length of its use cases\. This can be addressed by dividing the *Orchestrator* into a layer of services \(resulting in [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) or [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}})\) or multiple layers \(often yielding [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\)\. It is also possible to go for the [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) as that has more fine\-grained components\.
- *Small projects*\. The implementation overhead of defining and stabilizing service APIs and the performance penalty of the extra network hop may outweigh the extra flexibility of having the *Orchestrator* as a separate system component\.
- *Low latency*\. Any system\-wide use case will make multiple calls between the application \(*Orchestrator*\) and services, with each interaction adding to the latency\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Orchestrator.png" style="outline:none">
<img src="/Relations/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

*Orchestrator*:

- Extends [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) or, rarely, [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), or [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) \(forming *Layers*\)\.
- Can be merged with a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) into an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}), with a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) into an [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}), or with a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and [*Adapters*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) into an [*Enterprise Service Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\.
- Is a special case \(single service\) of [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) or \(2\-layer\) [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\.
- Can be implemented by a [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}})\.


## Variants by transparency

It seems that an *Orchestrator*, just like a [*layer*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), which it is, [can be]({{< relref "../part-2--basic-metapatterns/layers.md#dependencies" >}}) *open* \(*relaxed*\) or *closed* \(*strict*\):

### Closed or strict

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Closed.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Closed.png" alt="Orchestrator - Closed" style="width:90%"/>
</a>
</figure>

A *strict* or *closed Orchestrator* isolates the orchestrated services from their users – all the requests go through the *Orchestrator*, and the services don’t need to intercommunicate\. 

### Open or relaxed

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Open.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Open.png" alt="Orchestrator - Open" style="width:92%"/>
</a>
</figure>

An *open Orchestrator* implements a subset of system\-wide scenarios that require strict data consistency while less demanding requests go from the clients directly to the underlying services, which rely on [*choreography*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) or [*shared data*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/shared-data.md" >}}) for communication\. Such a system sacrifices the clarity of design to avoid some of the drawbacks of both *choreography* and *orchestration*:

- The orchestrator development team, which may be overloaded or slow to respond, is not involved in implementing the majority of use cases\.
- Most of the use cases avoid the performance penalty caused by the orchestration\.
- Failure of the *Orchestrator* does not disable the entire system\.
- The relaxed *Orchestrator* still allows for synchronized changes of data in multiple services, which is rather hard to achieve with choreography\.


## Variants by structure \(can be combined\)

The orchestration \(application \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] / [integration](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md) / [composite](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md)\) layer has several structural \(implementation\) options:

### Monolithic

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Monolythic.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Monolythic.png" alt="Orchestrator - Monolythic" style="width:42%"/>
</a>
</figure>

A single *Orchestrator* is deployed\. This option fits ordinary medium\-sized projects\.

### Scaled

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Scaled.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Scaled.png" alt="Orchestrator - Scaled" style="width:100%"/>
</a>
</figure>

High availability requires multiple [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of a stateless *Orchestrator* to be deployed\. A *Mediator* \([*Saga*]({{< relref "#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}), writing *Orchestrator*\) may store the current transaction’s state in a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) to assure that if it crashes there is always another instance ready to take up its job\.

High load systems also require multiple instances of *Orchestrators* because a single instance is not enough to handle the incoming traffic\.

<aside>

> Not all data is made equal\. For example, an online store has different requirements for reliability of its buyers’ cart contents and the payments\. If the current buyers’ carts become empty when the store’s server crashes, that makes only a minor annoyance\. However, any financial data loss or a corrupted money transfer is a real trouble\. Therefore an online store may implement its cart with a simple in\-memory *Orchestrator*, but should be very careful about the payments workflow, every step of which must be persisted to a reliable database\. 

</aside>

### Layered

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Layered.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Layered.png" alt="Orchestrator - Layered" style="width:100%"/>
</a>
</figure>

\[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] describes an option of a layered [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}})\. A client’s request comes to the topmost layer of the *Orchestrator* which uses the simplest \(and least flexible\) framework\. If the request is found to be complex, it is forwarded to the second layer which is based on a more powerful technology\. And if it fails or requires a human decision then it is forwarded again to the even more complex custom\-tailored orchestration layer\.

That allows the developers to gain the benefits of a high\-level declarative language in a vast majority of scenarios while falling back to hand\-written code for a few complicated cases\. The choice is not free as programmers need to learn multiple technologies, interlayer debugging is anything but easy and performance is likely to be worse than that of a monolithic *Orchestrator*\.

A similar example is using an [*API Composer*]({{< relref "#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) for the top layer, followed by a [*Process Manager*]({{< relref "#process-manager-orchestrator" >}}) and a [*Saga Engine*]({{< relref "#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}})\.

### A service per client type \(Backends for Frontends\)

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20BFF.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20BFF.png" alt="Orchestrator - BFF" style="width:100%"/>
</a>
</figure>

If your clients strongly differ in workflows \(e\.g\. [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) and [OLTP](https://en.wikipedia.org/wiki/Online_transaction_processing), or user and admin interfaces\), implementing dedicated *Orchestrators* is an option to consider\. That makes each client\-specific *Orchestrator* smaller and more cohesive than the unified implementation would be and gives more independence to the teams responsible for different kinds of clients\.

This pattern is known as [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) and has a chapter of its own\.

### A service per subdomain \(Hierarchy\)

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20Hierarchy.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20Hierarchy.png" alt="Orchestrator - Hierarchy" style="width:100%"/>
</a>
</figure>

In large systems a single *Orchestrator* is very likely to become overgrown and turn into a development bottleneck \(see [*Enterprise Service Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\)\. Building a [*hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) of *Orchestrators* may help \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], but only if the domain itself is hierarchical\. The top\-level component may even be a [*Reverse Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}) if no use cases cross subdomain borders or if the sub\-orchestrators employ [choreography]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}), resulting in a flat [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}})\. Otherwise it is a tree\-like [*Orchestrator of Orchestrators*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.

### A service per use case \(SOA\-style\)

<figure style="text-align:center">
<a href="/Variants/2/Orchestrator%20-%20SOA.png" style="outline:none">
<img src="/Variants/2/Orchestrator%20-%20SOA.png" alt="Orchestrator - SOA" style="width:100%"/>
</a>
</figure>

\[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] advises for single\-purpose *Orchestrators* in [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}): each *Orchestrator* manages one use case\. This enables fine\-grained scalability but will quickly lead to integration hell as new scenarios keep getting added to the system\. Overall, such a use of *Orchestrators* resembles the [*task layer*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) of [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\.

## Variants by function

*Orchestrators* may function in slightly different ways:

### API Composer, Remote Facade, Gateway Aggregation, Composed Message Processor, Scatter\-Gather, MapReduce

<figure style="text-align:center">
<a href="/Variants/2/API%20Composer.png" style="outline:none">
<img src="/Variants/2/API%20Composer.png" alt="API Composer" style="width:100%"/>
</a>
</figure>

*API Composer* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] is a kind of *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] which decreases the system’s latency by translating a high\-level incoming message into a set of lower\-level internal messages, sending them to the corresponding [services]({{< relref "../part-2--basic-metapatterns/services.md" >}}) in parallel, waiting for results and collecting the latter into a response to the original message\. Such a logic may often be defined declaratively in a third\-party tool without writing any low\-level code\. *Remote Facade* \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] is a similar pattern which makes synchronous calls to the underlying components – it exists to implement a coarse\-grained protocol with the system’s clients, so that a client may achieve whatever it needs through a single request\. [*Gateway Aggregation*](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation) is a generalization of these patterns\.

*Composed Message Processor* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] disassembles *API Composer* into smaller components: it uses a *Splitter* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] to subdivide the request into smaller parts, a *Router* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] to send each part to its recipient, and an *Aggregator* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] to collect the responses into a single message\. Unlike *API Composer*, it can also address [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) or [*Replicas*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}})\. A [*Scatter\-Gather*](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}}), [DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] broadcasts a copy of the incoming message to each recipient, thus it lacks a *Splitter* \(though \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] seems to ignore this difference\)\. [*MapReduce*](https://en.wikipedia.org/wiki/MapReduce) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] is similar to *Scatter\-Gather* except that it summarizes the results received in order to yield a single value instead of concatenating them\.

If an *API Composer* needs to conduct sequential actions \(e\.g\. first get user id by user name, then get user data by user id\), it becomes a *Process Manager* which may require some coding\.

An *API Composer* is usually deployed as a part of an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}})\.

Example: Microsoft has an [article](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation) on aggregation\.

### Process Manager, Orchestrator

<figure style="text-align:center">
<a href="/Variants/2/Process%20Manager.png" style="outline:none">
<img src="/Variants/2/Process%20Manager.png" alt="Process Manager" style="width:100%"/>
</a>
</figure>

*Process Manager* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] \(referred simply as *Orchestrator* in \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]\) is a kind of *Facade* that translates high\-level tasks into sequences of lower\-level steps\. This subtype of *Orchestrator* receives a client request, stores its state, runs pre\-programmed request processing steps, and returns a response\. Each of the steps of a *Process Manager* is similar to a whole task of an *API Composer* in that it generates a set of parallel requests to internal services, waits for the results and stores them for the future use in the following steps or final response\. The scenarios it runs may branch on conditions\.

A *Process Manager* may be implemented in a general\-purpose programming language, a declarative description for a third\-party tool, or a mixture thereof\.

A *Process Manager* is usually a part of an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}), [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}) or [*Enterprise Service Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\.

Example: \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] provides several examples\.

### \(Orchestrated\) Saga, Saga Orchestrator, Saga Execution Component, Transaction Script, Coordinator

<figure style="text-align:center">
<a href="/Variants/2/Saga.png" style="outline:none">
<img src="/Variants/2/Saga.png" alt="Saga" style="width:100%"/>
</a>
</figure>

*\(Orchestrated* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\]*\) Saga* \[[LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\], *Saga Orchestrator* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] or [*Saga Execution Component*](https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf) is a subtype of *Process Manager* which is specialized in *distributed transactions*\.

An *Atomically Consistent Saga* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] \(which is the default meaning of the term\) comprises a pre\-programmed sequence of \{“do”, “undo”\} action pairs\. When it is run, it iterates through the “do” sequence till it either completes \(meaning that the transaction succeeded\) or fails\. A failed *Atomically Consistent Saga* begins iterating through its “undo” sequence to roll back the changes that were already made\. In contrast, an *Eventually Consistent Saga* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] always retries its writes till all of them succeed\.

A *Saga* is often programmed declaratively in a third\-party [*Saga Framework*]({{< relref "../part-5--implementation-metapatterns/microkernel.md#saga-engine" >}}) which can be integrated into any service that needs to run a *distributed transaction*\. However, it is quite likely that such a service itself is an [*Integration Service*]({{< relref "#integration-micro-service-application-service" >}}) as it seems to [orchestrate]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) other services\.

A *Saga* plays the roles of both *Facade* by translating a single transaction request into a series of calls to the services’ APIs and *Mediator* by keeping the states of the services consistent \(the transaction succeeds or fails as a whole\)\. Sometimes a *Saga* may include requests to external services \(which are not parts of the system you are developing\)\.

A *Transaction Script* \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}}), [LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\] is a procedure that executes a transaction, possibly over multiple databases \[[LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\]\. Unlike a *Saga*, it is synchronous, written in a general programming language, and does not require a dedicated framework to run\. It operates database\(s\) directly while a *Saga* usually sends commands to services\. A *Transaction Script* may return data to its caller\.

*Coordinator* \[[POSA3]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa3" >}})\] is a generalized pattern for a component which manages multiple tasks \(e\.g\. software updates of multiple components\) to achieve “all or nothing” results \(if any update fails, other components are rolled back\)\.

Example: \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}})\] investigates many kinds of *Sagas* while \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] has a shorter description\.

### Integration \(Micro\-\)Service, Application Service

<figure style="text-align:center">
<a href="/Variants/2/Integration%20Service.png" style="outline:none">
<img src="/Variants/2/Integration%20Service.png" alt="Integration Service" style="width:100%"/>
</a>
</figure>

An [*Integration Service*](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md) is a full\-scale service \(often with a dedicated database\) that runs high\-level scenarios while delegating the bulk of the work to several other services \(remarkably, delegating to a single component forms [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}})\)\. Though an *Integration Service* usually features both functions of *Orchestrator*, in a [*control* system]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) its *Mediator* role is more prominent while in [*processing* software]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}}) it is going to behave more like the *Facade*\. A system with an *Integration Service* often resembles a shallow [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.

Example: Order Service in \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] seems to fit the description\.

## Variants of composite patterns

Several composite patterns involve an *Orchestrator* and are dominated by its behavior:

### API Gateway

<figure style="text-align:center">
<a href="/Variants/2/API%20Gateway.png" style="outline:none">
<img src="/Variants/2/API%20Gateway.png" alt="API Gateway" style="width:100%"/>
</a>
</figure>

An *API Gateway* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] is a component that processes client requests \(and encapsulates an implementation of a client protocol\(s\)\) as a [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(a kind of [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\) but also splits every client request into multiple requests to internal services as an *API Composer* or *Process Manager* \(which are *Orchestrators*\)\. It is a common pattern for backend solutions as it provides all the means to isolate the stable core of the system’s implementation from its fickle clients\. Usually a third\-party framework implements and colocates both its aspects, namely *Proxy* and *Orchestrator*, thus simplifying deployment and improving latency\.

Example: a thorough article from [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway)\.

### Event Mediator

<figure style="text-align:center">
<a href="/Variants/2/Event%20Mediator.png" style="outline:none">
<img src="/Variants/2/Event%20Mediator.png" alt="Event Mediator" style="width:100%"/>
</a>
</figure>

*Event Mediator* \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is an *orchestrating* [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. It not only receives requests from clients and turns each request into a multistep use case \(as a *Process Manager*\) but it also manages the deployed instances of services and acts as a medium which transports requests to the services and receives confirmations from them\. Moreover, unlike an ordinary *Middleware*, it seems to be aware of all of the kinds of messages in the system and which service each message must be forwarded to, resulting in overwhelming complexity concentrated in a single component which does not even follow the principle of separation of concerns\. \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] recommends building a hierarchy of *Event Mediators* from several vendors, further complicating the architecture\.

Example: Mediator Topology in the chapter of \[[FSA](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.d09ykbr4tzvn)\] on Event\-Driven Architecture\.

### Enterprise Service Bus \(ESB\)

<figure style="text-align:center">
<a href="/Variants/2/Enterprise%20Service%20Bus.png" style="outline:none">
<img src="/Variants/2/Enterprise%20Service%20Bus.png" alt="Enterprise Service Bus" style="width:100%"/>
</a>
</figure>

[*Enterprise Service Bus*](https://www.confluent.io/learn/enterprise-service-bus/) \(*ESB*\) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is an overgrown *Event Mediator* that incorporates lots of [*cross\-cutting concerns*](https://en.wikipedia.org/wiki/Cross-cutting_concern), including protocol translation for which it utilizes at least one [*Adapter*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per service\. The combination of a central role in organizations and its complexity was among the main reasons for the demise of [*Enterprise Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}})\.

Example: Orchestration\-Driven Service\-Oriented Architecture in \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]\.

## Evolutions

Employing an *Orchestrator* has two pitfalls:

- The system becomes slower because too much communication is involved\.
- A single *Orchestrator* may grow too large and rigid\.


There is [one way to counter the first point and more than one to solve the second]({{< relref "../part-7--appendices/appendix-e--evolutions/evolutions-of-an-orchestrator.md" >}}):

- Subdivide the *Orchestrator* by the system’s subdomains, forming [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) and minimizing network communication\.


<figure style="text-align:center">
<a href="/Evolutions/2/Orchestrator%20to%20Layered%20Services.png" style="outline:none">
<img src="/Evolutions/2/Orchestrator%20to%20Layered%20Services.png" alt="Orchestrator to Layered Services" style="width:100%"/>
</a>
</figure>

- Subdivide the *Orchestrator* by the type of client, forming [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/2/Orchestrator%20to%20Backends%20for%20Frontends.png" style="outline:none">
<img src="/Evolutions/2/Orchestrator%20to%20Backends%20for%20Frontends.png" alt="Orchestrator to Backends for Frontends" style="width:100%"/>
</a>
</figure>

- Add another [*layer*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) of orchestration\.


<figure style="text-align:center">
<a href="/Evolutions/2/Orchestrator%20add%20Orchestrator.png" style="outline:none">
<img src="/Evolutions/2/Orchestrator%20add%20Orchestrator.png" alt="Orchestrator add Orchestrator" style="width:100%"/>
</a>
</figure>

- Build a [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/2/Orchestrator%20to%20Hierarchy.png" style="outline:none">
<img src="/Evolutions/2/Orchestrator%20to%20Hierarchy.png" alt="Orchestrator to Hierarchy" style="width:100%"/>
</a>
</figure>

## Summary

 An *Orchestrator* distills the high\-level logic of your system and keeps it together in a layer which integrates other components\. However, the separation of business logic into two layers results in much communication which impairs performance\.

<nav>

| \<\< [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) | ^ [Part 3\. Extension metapatterns]({{< relref "../part-3--extension-metapatterns/_index.md" >}}) ^ | [Combined Component]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}) \>\> |
| --- | --- | --- |

</nav>



