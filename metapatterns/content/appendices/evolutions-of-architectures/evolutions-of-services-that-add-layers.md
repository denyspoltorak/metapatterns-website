+++
weight = 12
title = "Evolutions of Services that add layers"
description = "A system of services can be extended with a Middleware, Service Mesh, Proxies, Shared Database, or an Orchestrator which implement cross-cutting concerns."
[sitemap]
  priority = 0.3
+++

# Evolutions of Services that add layers {anchor=false}

The most common modifications to a [system of *Services*]({{< relref "../../basic-metapatterns/services.md" >}}) involve supplementary system\-wide *layers* which compensate for the inability of the *Services* to share anything among themselves:

- A [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) knows of all the deployed service [instances]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}})\. It mediates communication between them and may manage their scaling and failure recovery\.
- [*Sidecars*]({{< relref "../../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../../appendices/books-referenced.md#dds" >}})\] of a [*Service Mesh*]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}) make a virtual layer of [shared libraries]({{< relref "../../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}) for the [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}) it hosts\.
- A [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) simplifies the initial phases of development and provides data consistency and [interservice communication]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\.
- [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}}) stand between the system and its clients and take care of shared aspects that otherwise would need to be implemented by every service\.
- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) is the single place for the high\-level logic of every use case\.


Those layers may also be merged into [*Combined Components*]({{< relref "../../extension-metapatterns/combined-component.md" >}}):

- [*Message Bus*]({{< relref "../../extension-metapatterns/combined-component.md#message-bus" >}}) is a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) that supports multiple protocols\.
- [*API Gateway*]({{< relref "../../extension-metapatterns/combined-component.md#api-gateway" >}}) combines [*Gateway*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(a kind of [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}})\) and [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.
- [*Event Mediator*]({{< relref "../../extension-metapatterns/combined-component.md#event-mediator" >}}) is an [*orchestrating*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}})\.
- [*Shared Event Store*]({{< relref "../../extension-metapatterns/combined-component.md#persistent-event-log-shared-event-store" >}}) combines [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) and [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\.
- [*Enterprise Service Bus \(ESB\)*]({{< relref "../../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) is an [*orchestrating*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) [*Message Bus*]({{< relref "../../extension-metapatterns/combined-component.md#message-bus" >}})\.
- [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) employs all the four layers: [*Gateway*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) and [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}})\.


## Add a Middleware

<figure>
<a href="/diagrams/Evolutions/Services/Services%20add%20Middleware.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services%20add%20Middleware.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services%20add%20Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services%20add%20Middleware.png" alt="Services add Middleware" loading="lazy" width="1307" height="304" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Middleware]({{< relref "../../extension-metapatterns/middleware.md" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: take care of scaling, recovery, and interservice communication without programming it\.

<ins>Prerequisite</ins>: communication between the services is uniform\.

Distributed systems may fail in a zillion ways\. You want to ruminate neither on that nor on [heisenbugs](https://en.wikipedia.org/wiki/Heisenbug)\. And you probably want to have a framework for scaling the services and restarting them after failure\. Get a third\-party *Middleware*\! Let your programmers write the business logic, not infrastructure\.

<ins>Pros</ins>: 

- You don't invest your time in infrastructure\.
- Scaling and error recovery are made easy\.


<ins>Cons</ins>: 

- There may be a performance penalty which becomes worse for uncommon patterns of communication\.
- The *Middleware* may be a single point of failure\.


<ins>Further steps</ins>:

- Use a [*Service Mesh*]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}) for dynamic scaling and as a way to implement [shared aspects]({{< relref "../../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}})\.


## Use a Service Mesh

<figure>
<a href="/diagrams/Variants/2/Multifunctional%20-%20Service%20Mesh.png">
<picture>
<source srcset="/diagrams/Variants/2/Multifunctional%20-%20Service%20Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Multifunctional%20-%20Service%20Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Multifunctional%20-%20Service%20Mesh.png" alt="Multifunctional - Service Mesh" loading="lazy" width="1083" height="323" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Service Mesh]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}) \([Mesh]({{< relref "../../implementation-metapatterns/mesh.md" >}}), [Middleware]({{< relref "../../extension-metapatterns/middleware.md" >}})\), [Sidecar]({{< relref "../../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \([Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}})\), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: support dynamic scaling and interservice communication out of the box; share libraries among the services\.

<ins>Prerequisite</ins>: service instances are mostly [stateless]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}})\.

The [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}) architecture boasts dynamic scaling under load thanks to its *Mesh*\-based *Middleware*\. It also allows for the services to share libraries in *Sidecars* \[[DDS]({{< relref "../../appendices/books-referenced.md#dds" >}})\] – additional containers co\-located with each service instance – to avoid duplication of generic code among the services\.

<ins>Pros</ins>: 

- Dynamic scaling and error recovery\.
- Available out of the box\.
- Provides a way to implement shared aspects \(cross\-cutting concerns\) once and use the resulting libraries in every service\.


<ins>Cons</ins>: 

- Performance degrades because of the complex distributed infrastructure\.
- You may suffer vendor lock\-in\.


## Use a Shared Repository

<figure>
<a href="/diagrams/Evolutions/Services/Services%20to%20Shared%20Database.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services%20to%20Shared%20Database.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services%20to%20Shared%20Database.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services%20to%20Shared%20Database.png" alt="Services to Shared Database" loading="lazy" width="1267" height="284" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: let the services [share data]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}}), don’t invest in operating multiple databases\.

<ins>Prerequisite</ins>: the services use a uniform approach to persisting their data\.

You don’t really need every service to have a private database\. A shared one is enough in many cases\.

<ins>Pros</ins>: 

- It is easy for the services to share and synchronize data\.
- Lower operational complexity\.


<ins>Cons</ins>: 

- All the services depend on the database schema which becomes hard to alter\.
- The single database will limit performance of the system\.
- It may also become a single point of failure\.


<ins>Further steps</ins>:

- [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) scales the data layer but it is a simple key\-value store\.
- [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) is about having multiple specialized databases\.


## Add a Proxy

<figure>
<a href="/diagrams/Evolutions/Services/Services%20add%20Proxy.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services%20add%20Proxy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services%20add%20Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services%20add%20Proxy.png" alt="Services add Proxy" loading="lazy" width="1307" height="385" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: use a common infrastructure component on behalf of your entire system\.

<ins>Prerequisite</ins>: the system serves its clients in a uniform way\.

Putting a generic component between the system and its clients helps the programmers concentrate on business logic rather than protocols, infrastructure or even security\.

<ins>Pros</ins>: 

- You get a choice of generic functionality without investing development time\.
- It is an additional layer that isolates your system from both clients and attackers\.


<ins>Cons</ins>: 

- There is a latency penalty caused by the extra network hop\.
- Each *Proxy* may be a single point of failure or at least needs some admin oversight\.


<ins>Further steps</ins>:

- You can always add another kind of [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}})\.
- If there are multiple clients that differ in their protocols, you can employ a stack of *Proxies* per client, resulting in [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


## Use an Orchestrator

<figure>
<a href="/diagrams/Evolutions/Services/Services%20use%20Orchestrator.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services%20use%20Orchestrator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services%20use%20Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services%20use%20Orchestrator.png" alt="Services use Orchestrator" loading="lazy" width="1307" height="385" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: have the high\-level logic of use cases distilled as intelligible code\.

<ins>Prerequisite</ins>: the use cases comprise sequences of high\-level steps \(which is very likely to be true for a system of [*subdomain services*]({{< relref "../../basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}})\)\.

When a use case jumps over several services in a dance of [*choreography*]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}}), there is no easy way to understand it as there is no single place to see it in the code\. It may be even worse with [*Pipelined*]({{< relref "../../basic-metapatterns/pipeline.md" >}}) systems where use cases are embodied in the structure of event channels between the components\.

Extract the high\-level business logic from the choreographed services or their interconnections and put it into a dedicated component\.

<ins>Pros</ins>: 

- You are not limited in the number and complexity of use cases anymore\.
- Global use cases become much easier to debug\.
- You have a new team dedicated to the interaction with customers, freeing the other teams to study their parts of the domain or work on improvements\.
- Many changes in the high\-level logic can be implemented and deployed without touching the main services\.
- The extra layer decouples the main services from the system’s clients and from each other\.


<ins>Cons</ins>: 

- There is a performance penalty because the number of messages per use case doubles\.
- The *Orchestrator* may become a single point of failure\.
- Some flexibility is lost as the *Orchestrator* couples qualities of the services\.


<ins>Further steps</ins>:

- If there are several clients that strongly vary in workflows, you can apply [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) with an *Orchestrator* per client\.
- If the *Orchestrator* grows too large, it [can be divided]({{< relref "../../extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers, services or both, the latter option resulting in a [*Top\-Down Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.
- The *Orchestrator* can be [scaled]({{< relref "../../extension-metapatterns/orchestrator.md#scaled" >}}) and can have its own database\.
