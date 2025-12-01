+++
weight = 8
title = "Evolutions of Layers that help large projects"
description = "When layered software grows large, its business logic should be subdivided into Services, Pipeline, or Hierarchy."
images = ["/diagrams/Web/og/Favicon-plain.png"]
[sitemap]
  priority = 0.3
+++

# Evolutions of Layers that help large projects {anchor=false}

The main drawback \(and benefit\) of [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) is that much or all of the business logic is kept together in one or two components\. That allows for easy debugging and fast development in the initial stages of the project but slows down and complicates work as the project [grows in size]({{< relref "../../analytics/architecture-and-product-life-cycle.md#youth-development-of-features--fragmented-architectures" >}})\. The only way for a growing project to survive and continue evolving at a reasonable speed is to divide its business logic into several smaller, thus less [complex]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md" >}}), components that match subdomains \(*bounded contexts* \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\]\)\. There are several options for such a change whose applicability depends on the domain:

- The middle layer with the main business logic can be divided into [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) leaving the upper [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) and lower [*database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) layers intact for future evolutions\.
- Sometimes the business logic can be represented as a set of directed graphs which is known as [*Event\-Driven Architecture*]({{< relref "../../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}})\.
- If you are lucky, your domain is naturally a [*Top\-Down Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


## Divide the domain layer into Services

<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.png" alt="Layers Split Domain to Services" loading="lazy" width="1083" height="243" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Cell]({{< relref "../../extension-metapatterns/combined-component.md#inexact-cell-wso2-definition" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}}), [Shared Database]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\), [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: make the code simpler and let several teams work on the project efficiently\.

<ins>Prerequisite</ins>: the low\-level business logic comprises loosely coupled subdomains\.

It is very common for a system’s domain to consist of weakly interacting *bounded contexts* \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\]\. They are integrated through high\-level use cases and/or [relations in data]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\. For such a system it is relatively easy to divide the domain logic into *Services* while leaving the integration and data layers shared, yielding a *Cell*\.

<ins>Pros</ins>: 

- You get multiple specialized development teams\.
- The largest and most complex piece of code is split into several smaller components\.
- There is more flexibility with deployment and scaling\.


<ins>Cons</ins>: 

- Future changes in the overall structure of the domain will be harder to implement\.
- System\-wide use cases become somewhat harder to debug as they span over many components\.
- Performance may degrade if the *Services* and *Orchestrator* become distributed\.


<ins>Further steps</ins>:

- Continue by splitting the *Orchestrator* and *database*, resulting in [*Orchestrated Three\-Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}})\.
- Divide the *Orchestrator* \(by type of client\) into [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.
- Use multiple databases in [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Scale well with [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}})\.


## Build an Event\-Driven Architecture over a Shared Database

<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.png" alt="Layers Split to Event-Driven Architecture" loading="lazy" width="1166" height="264" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Event\-Driven Architecture]({{< relref "../../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) \([Pipeline]({{< relref "../../basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../../basic-metapatterns/services.md" >}})\)\), [Shared Database]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: untangle the code, support multiple teams, improve scalability\.

<ins>Prerequisite</ins>: use cases are sequences of loosely coupled coarse\-grained steps\.

If your system has a well\-defined workflow for processing every kind of input request, it can be divided into several [*subdomain services*]({{< relref "../../basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}}), each hosting a few related steps of multiple use cases\. Each service subscribes to inputs from other services and/or system’s clients and publishes output events\.

<ins>Pros</ins>: 

- The code is divided into much smaller \(and simpler\) segments\.
- It is easy to add new steps or use cases as the structure is quite flexible\.
- You open a way to having several almost independent teams, one per service\.
- You can achieve flexible deployment and scaling as the services are stateless, but you need a *Middleware* for that\.
- The architecture naturally supports event replay as the means of reproducing bugs or testing / benchmarking individual components\.
- There is no need for explicit scheduling or thread synchronization\.


<ins>Cons</ins>: 

- The system as a whole is hard to debug\.
- You will have to live with high latency\.
- You may end up with too many components which are interconnected in too many ways\.


<ins>Further steps</ins>:

- Add a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) that supports scaling and failure recovery\.
- Split the *Shared Database* by subdomain, yielding [*Choreographed Two\-Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md#choreographed-two-layered-services" >}})\.
- Scale with [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}})\.
- Extract the logic of use cases into an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.


## Build a Top\-Down Hierarchy

<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.png" alt="Layers to Hierarchy" loading="lazy" width="1103" height="249" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Top\-Down Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) \([Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}})\)\.

<ins>Goal</ins>: untangle the code, support multiple teams, earn fine\-grained scalability\.

<ins>Prerequisite</ins>: the domain is hierarchical\.

Splitting the lower layers into independent components with identical interfaces simplifies the managing code and allows the managed components to be deployed, developed, and run independently of each other\. Ideally, the mid\-layer components should participate in decision\-making so that the uppermost component is kept relatively simple\.

<ins>Pros</ins>: 

- Hierarchy is easy to develop and support with multiple teams\.
- Individual components are straightforward to modify or replace\.
- The components scale, deploy, and run independently\.
- The system is quite fault tolerant\.


<ins>Cons</ins>: 

- It takes time and skill to figure out good interfaces\.
- There are many components to administer\.
- Latency is suboptimal for system\-wide use cases\.
