+++
weight = 10
title = "Combined Component"
+++

# Combined Component

*Jack of all trades\.* Use third\-party software which covers multiple concerns\.

<ins>Aspects:</ins> those of the individual components it combines\.

<ins>Variants:</ins> 

- Message Bus \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\], 
- API Gateway \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\], 
- Event Mediator \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], 
- Persistent Event Log / Shared Event Store,
- Front Controller \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\],
- [Enterprise Service Bus](https://www.confluent.io/learn/enterprise-service-bus/) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\],
- Service Mesh \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\],
- Middleware of Space\-Based Architecture \[[SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sap" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]\.


<ins>Structure:</ins> Two or more \(usually\) extension patterns combined into a single component\.

<ins>Type:</ins> Extension\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Works off the shelf | Is yet another technology to learn |
| Improved latency | May not be flexible enough for your needs |
| Less DevOps effort | May become overcomplicated |


<ins>References:</ins> Mostly \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway) on *API Gateway*, \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\] on *Shared Event Store*\.

Two or three metapatterns may be blended together into a *Combined Component* which is usually a ready\-to\-use framework which tries to cover \(and subtly create\) as many project needs as possible to make sure it will never be dropped from the project\. On one hand, such a framework may provide a significant boost to the speed of development\. On the other – it is going to force you into its own area of applicability and keep you bound within it\.

### Performance

A *Combined Component* tends to improve performance as it removes the network hop\(s\) and data serialization between the components it replaces\. It is also likely to be highly optimized\. However, that matters if you really need all the functionality you are provided with, otherwise you may end up running a piece of software which is too complex and slow for the tasks at hand\.

### Dependencies

A *Combined Component* has all the dependencies of its constituent patterns\.

### Applicability

*Combined patterns* <ins>work well</ins> for:

- *Series of similar projects\.* If your team is experienced with the technology and knows its pitfalls, it will be used efficiently and safely\.
- *Small\- to medium\-sized domains\.* An off\-the\-shelf framework relieves you of infrastructure concerns\. No thinking, no decisions, no time wasted\.


*Combined patterns* <ins>are better avoided</ins> in:

- *Research and development\.* You may find that the technology chosen is too limiting or a wrong fit for your needs after it has already been deeply integrated into your code and infrastructure\.
- *Large projects*\. Most of the combined patterns include an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) which tends to grow unmanageably large \(requiring [some kind of division]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}})\) as the project advances\.
- *Long\-running projects*\. You are very likely to step right into *vendor lock\-in*\. The industry will evolve, leaving you with the obsolete technology you have chosen and integrated\.


## Variants

Combined components vary in their structure and properties:

### Message Bus

<figure>

<div style="text-align:center">
<img src="/Variants/2/Message%20Bus.png" alt="Message Bus" style="width:100%"/>
</div>

</figure>

A *Message Bus* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] is a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) which employs an [*Adapter*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) per [*service*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) allowing services that differ in protocols to intercommunicate\.

### API Gateway

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20API%20Gateway.png" alt="Multifunctional - API Gateway" style="width:100%"/>
</div>

</figure>

*API Gateway* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] is a component which processes client requests \(and encapsulates the client protocol\(s\)\) much like a [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(a kind of [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\) but it also splits each client request into multiple requests to the underlying services like an [*API Composer*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) or a [*Process Manager*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#process-manager-orchestrator" >}}) \([*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\)\.

If the orchestration logic of an *API Gateway* needs to become more complex, it makes sense to split it into a separate *Gateway* and *Orchestrator*, rewriting the latter as a custom [*Application Service*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#integration-micro-service-application-service" >}})\. When there are multiple types of clients that strongly differ in workflows and protocols, one *API Gateway* per client type is used, resulting in [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\. A [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) relies on *API Gateways* for isolation of its [*Cells*]({{< relref "../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.

Example: a thorough article from [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway)\.

### Event Mediator

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20Event%20Mediator.png" alt="Multifunctional - Event Mediator" style="width:100%"/>
</div>

</figure>

An *Event Mediator* \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is an *orchestrating* [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. It not only receives requests from clients and turns each request into a multi\-step use case \(as does [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) but it also manages instances of the services and acts as the medium which calls them\. Moreover, it seems to be aware of all the kinds of messages in the system and which service each message must be forwarded to, resulting in an overwhelming complexity concentrated in a single component which does not even follow the separation of concerns principle\. \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] proposes countering that by using multiple *Event Mediators* [split over the following dimensions]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}):

- Client applications or bounded contexts, dividing the *Event Mediator*’s responsibility by subdomain\.
- Complexity of a use case, with simple scenarios handled by a simple first\-line *Event Mediator* and the more complicated scenarios being forwarded to second\- and third\-line *Event Mediators* which employ advanced [*orchestration engines*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.


Either case, strangely, results in multiple *Middlewares* connected to the same set of [services]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

The *Event Mediator* pattern seems to be well established but, obviously, it may become quite messy for larger projects with nontrivial interactions\. Such cases may also be solved by separating the *Middleware* from the *Orchestrator* and [dividing the latter]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) or [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.

Example: Mediator Topology in the \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] chapter on Event\-Driven Architecture\.

### Persistent Event Log, Shared Event Store

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20Shared%20Event%20Store.png" alt="Multifunctional - Shared Event Store" style="width:100%"/>
</div>

</figure>

When a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) grants long\-term persistence, it becomes a [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\. This may happen to an *Event Log* which implements interservice communication\.

On the other hand, a shared *Event Store*, which persists changes of internal state of services and replaces the services’ databases, may be extended to store interservice events, taking the role of a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.

Either case makes changing the event schema troublesome because either all the services that read the event will need to support multiple \(historical\) schemas or you’ll have to overwrite the entire event history, translating the old schema into a new one\.

Example: \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\] shows this implemented with Apache Kafka\.

### Front Controller

<figure>

<div style="text-align:center">
<img src="/Variants/2/Front%20Controller.png" alt="Front Controller" style="width:100%"/>
</div>

</figure>

*Front Controller* \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#front-controller" >}}) [PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] is the name for the first \(client\-facing\) service of a [*pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) in [*Choreographed Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) when that service collects information about the status of each request it has processed and forwarded down the *pipeline\.* The status is received by listening for notifications from the downstream services and is readily available for the *Front Controller*’s clients, resembling the function of [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \([*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\) and [*Application Service*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#integration-micro-service-application-service" >}}) \([*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\)\.

### Enterprise Service Bus \(ESB\)

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20Enterprise%20Service%20Bus.png" alt="Multifunctional - Enterprise Service Bus" style="width:100%"/>
</div>

</figure>

An [*Enterprise Service Bus*](https://www.confluent.io/learn/enterprise-service-bus/) \(*ESB*\) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is a mixture of *Message Bus* \(with a stack of *Adapters* per service\) and *Event Mediator* \(built\-in *Orchestrator*\) which turns this kind of [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) into the core of the system\. The combination of a central role in organizations and its complexity was among the main reasons for the demise of [*Enterprise Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\.

Example: Orchestration\-Driven Service\-Oriented Architecture in \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\], [how it is born](http://memeagora.blogspot.com/2009/01/tactics-vs-strategy-soa-tarpit-of.html) and [how it dies](http://memeagora.blogspot.com/2009/03/triumph-of-hope-over-reason-soa-tarpit.html) by Neal Ford\.

### Service Mesh

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20Service%20Mesh.png" alt="Multifunctional - Service Mesh" style="width:100%"/>
</div>

</figure>

A *Service Mesh* \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) that employs one or more [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] per service as a place for *cross\-cutting concerns* \(logging, observability, encryption, protocol translation\)\. A service accesses its *Sidecar* without performance and stability penalties as they are running on the same machine\. The totality of deployed *Sidecars* makes a system\-wide virtual layer of shared libraries: though the *Sidecars* are physically separate, they are often identical and stateless, so that a service that accesses one *Sidecar* may be thought of as accessing all of them\.

A *Service* [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) is also responsible for dynamic scaling \(it creates new instances if the load increases and destroys them if they become idle\) and failure recovery of the services\. Last but not least, it provides a messaging infrastructure for the [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) to intercommunicate\.

### Middleware of Space\-Based Architecture

<figure>

<div style="text-align:center">
<img src="/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.png" alt="Multifunctional - Space-Based Architecture" style="width:100%"/>
</div>

</figure>

[*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) \[[SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sap" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] relies on the most complex *Middleware* known – it incorporates all four of the *extension metapatterns*:

- The *Messaging Grid* is a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) \(combination of [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Dispatcher*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\) that receives, preprocesses, and persists client requests\. Simple requests are forwarded to a less loaded *Processing Unit* \(service with business logic\) while complex ones go to the *Processing Grid*\.
- A *Processing Grid* is an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) which manages multi\-step workflows for complex requests\.
- A [*Data Grid*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is a [distributed]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) in\-memory [database]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\. It is built of caching nodes which are co\-located with instances of *Processing Units*, making the database access extremely fast\. However, the speed and scalability is paid for with stability – any data in RAM is prone to disappearing\. Therefore the *Data Grid* backs up all the changes to a slower *Persistent Database*\.
- A *Deployment Manager* is a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) that creates and destroys instances of *Processing Units* \(which are paired to the nodes of the *Data Grid*\), just like [*Service Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) does for [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) \(which are paired to [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\]\)\. However, in contrast to *Service Mesh*, it does not provide a messaging infrastructure because *Processing Units* communicate by [sharing data]({{< relref "../part-1--foundations/arranging-communication/shared-data.md" >}}) via the *Data Grid*, not by sending messages\.


The four layers of the *Space\-Based Architecture*’s *Middleware* are reasonably independent\. Together they make a system that is both more scalable and more complex than *Microservices*\.

## Evolutions

The patterns that involve [*orchestration*]({{< relref "../part-1--foundations/arranging-communication/orchestration.md" >}}) \(*API Gateway*, *Event Mediator* and *Enterprise Service Bus*\) may allow for most of the evolutions of [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) by deploying multiple instances of the *Combined Component* which differ in orchestration logic\. There is also one unique evolution:

- Replace the *Combined Component* with several specialized ones


<figure>

<div style="text-align:center">
<img src="/Evolutions/2/Multifunctional_%20Split.png" alt="Multifunctional: Split" style="width:100%"/>
</div>

</figure>

## Summary

A *Combined Component* is a ready\-to\-use framework that may speed up development but will likely constrain your project to follow its guidelines with no regard to your real needs\.

<nav>

| \<\< [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) | ^ [Part 3\. Extension Metapatterns]({{< relref "../part-3--extension-metapatterns/_index.md" >}}) ^ | [Part 4\. Fragmented Metapatterns]({{< relref "../part-4--fragmented-metapatterns/_index.md" >}}) \>\> |
| --- | --- | --- |

</nav>



