+++
weight = 2
title = "Evolutions of a Monolith that result in Layers"
description = "A Monolith can be split into Layers, or a specialized layer, such as a Proxy or an Orchestrator, may be added between an existing Monolith and its clients."
[sitemap]
  priority = 0.3
+++

# Evolutions of a Monolith that result in Layers

Another drawback of [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) is its … er … monolithism\. The entire application exposes a single set of qualities and all its parts \(if they ever emerge\) are deployed together\. However, life awards flexibility: parts of a system may benefit from being written in varying languages and styles, deployed with different frequency and amount of testing, sometimes to specific hardware or end users’ devices\. They may need to [vary in security and scalability]({{< relref "../../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#distribution" >}}) as well\. Enter [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) – a subdivision by the *level of abstractness*:

- Most *Monoliths* can be divided into three or four [*layers*]({{< relref "../../basic-metapatterns/layers.md" >}})\.
- It is common to see the database separated from the main application\.
- [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}}) \(e\.g\. [*Firewall*]({{< relref "../../extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), [*Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Reverse Proxy*]({{< relref "../../extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}})\) are common additions to the system\.
- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) adds a layer of indirection to simplify the system’s API for its clients\.


## Divide into Layers

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers.png" alt="Monolith to Layers" loading="lazy" width="1087" height="285" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: let parts of the system vary in qualities, improve the structure of the code\.

<ins>Prerequisite</ins>: there is a natural way to separate the high\-level logic from the low level implementation details and dependencies\.

Most systems apply *layering* by default as it grants a lot of flexibility at very little cost\. [Common sets of layers]({{< relref "../../basic-metapatterns/layers.md#examples" >}}) are: UI, tasks \(orchestration\), domain \(detailed business rules\) and infrastructure \(database and libraries\) or frontend, backend and data\.

<ins>Pros</ins>:

- It is a natural way to specialize and decouple two or three development teams\.
- The layers may vary in virtually any quality:
  - They are deployed and scaled independently\.
  - They may run on different hardware, including client devices\.
  - They may vary in programming language, paradigm and release cycle\.
- Most changes are isolated to a single layer\.
- Layering opens a way to many evolutions of the system\.
- The code [becomes easier to read]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md" >}})\.


<ins>Cons</ins>: 

- Dividing an existing application into *Layers* may take some effort\.
- There is a small performance penalty\.


## Use a database

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20add%20Database.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Database.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Database.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20add%20Database.png" alt="Monolith add Database" loading="lazy" width="1107" height="244" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}}), [Shared Database]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \([Shared Repository]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\)\.

<ins>Goal</ins>: avoid implementing a datastore\.

<ins>Prerequisite</ins>: the system needs to query \(maybe also persist\) a large amount of data\.

A datastore is non\-trivial to implement\. While ordinary files are good for small volumes of data, as your needs grow so needs to grow your technology\. Deploy a database\.

<ins>Pros</ins>: 

- A well\-known database is sure to be more reliable than any in\-house implementation\.
- Many databases provide heavily optimized algorithms for querying data\.
- You can choose other hardware to deploy the database to\.
- Your \(now stateless\) application will be easy to scale\.


<ins>Cons</ins>: 

- Databases are complex and require fine\-tuning\.
- You cannot adapt the database engine to your evolving needs\.
- Most databases do not scale\.
- You are stepping right into [vendor lock\-in](https://en.wikipedia.org/wiki/Vendor_lock-in)\.


<ins>Further steps</ins>:

- Deploy multiple [*instances*]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of your application behind a [*Load Balancer*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\.
- Continue the transition to [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) by separating the high\-level and low\-level business logic\.
- [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) improves performance of the data layer\.
- [*CQRS*]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) passes read and write requests through dedicated services\. 
- [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is low latency and allows for dynamic scalability of the whole system, including the data layer\.
- [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) will allow you to switch to another database in the future\.


## Add a Proxy

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20add%20Proxy.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Proxy.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20add%20Proxy.png" alt="Monolith add Proxy" loading="lazy" width="1087" height="361" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}}), [Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}})\.

<ins>Goal</ins>: avoid implementing generic functionality\.

<ins>Prerequisite</ins>: Your system serves clients \(as opposed to [controlling hardware]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\)\.

A *Proxy* is placed between your system and its clients to provide generic functionality that otherwise would have to be implemented by the system\. The kinds of *Proxy* to use with [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) are: [*Firewall*]({{< relref "../../extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), [*Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Reverse Proxy*]({{< relref "../../extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and [*Adapter*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\. Multiple *Proxies* can be deployed\.

<ins>Pros</ins>: 

- You save some time \(and money\) on development\.
- A well\-known *Proxy* is likely to be more secure and reliable than an in\-house implementation\.
- You can select hardware to deploy the *Proxy* to\.


<ins>Cons</ins>: 

- Latency degrades, except for [*Response Cache*]({{< relref "../../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) where it depends on frequency of requests\.
- The *Proxy* may fail, which increases the chance of failure of your system\.
- Beware of [vendor lock\-in](https://en.wikipedia.org/wiki/Vendor_lock-in)\.


<ins>Further steps</ins>:

- Another kind of *Proxy* may be added\.
- Some systems employ a *Proxy* per client, leading to [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


## Add an Orchestrator

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20add%20Orchestrator.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Orchestrator.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20add%20Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20add%20Orchestrator.png" alt="Monolith add Orchestrator" loading="lazy" width="1047" height="323" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}}), [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.

<ins>Goal</ins>: provide a high\-level API for clients to improve their developer experience and performance\.

<ins>Prerequisite</ins>: the API of your system is fine\-grained and there are common use cases which repeat certain sequences of calls to your API\.

A well\-designed *Orchestrator* should provide a high\-level API which is intuitive, easy to use, and coarse\-grained to minimize the number of interactions between the system and its clients\. An old way to access the original system’s API may still be maintained for rare use cases or legacy client applications \([*open* orchestration]({{< relref "../../extension-metapatterns/orchestrator.md#open-or-relaxed" >}})\)\. As a matter of fact, you program common logic on behalf of your clients\.

<ins>Pros</ins>: 

- Client applications become easier to write\. 
- Latency improves\.


<ins>Cons</ins>: 

- You get yet another moving part to design, test, deploy, and observe; and lots of meetings between development teams for a bonus\.
- The new coarse\-grained interface will likely be less powerful than the original one\.


<ins>Further steps</ins>:

- [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) use an *Orchestrator* per client type\.


## Further steps

Applying one of the evolutions discussed above does not prevent you from following another one of them, or even the same one for a second time:

- A layer can be split into two layers\.
- A database can be added\.
- Multiple kinds of *Proxies* are OK\.
- If you don’t have an *orchestration layer* yet, you may add one\.


Those were evolutions from *Layers* to [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}})\.

Another set of evolutions stems from splitting one or more *layers* into [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}):

- Splitting a [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) yields [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) where requests from each kind of client are processed by a dedicated component\.
- Splitting the layer with the main business logic results in [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}), possibly augmented with layers of [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}), [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}), [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.
- Splitting the *database layer* leads to [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) with specialized storages\.
- If all the layers share the domain dimension and are split along it, [*Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md" >}}) \(or its subtype [*CQRS*]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\) emerge\.
- If each layer is split along its own domain, the system follows [*Service\-Oriented Architecture*]({{< relref "../../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) that is built around component reuse\.
- Finally, some domains support [*Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}) – a tree\-like architecture where each layer takes a share of the system’s functionality\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%201.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%201.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%201.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%201.png" alt="Monolith to Layers - Further 1" loading="lazy" width="1126" height="843" style="width:100%"/>
</picture>
</a>
</figure>

In addition,

- Distributed systems usually allow for the [scaling]({{< relref "../../basic-metapatterns/shards.md" >}}) of one or more layers\.
- A layer may employ [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) for better customizability\.
- The UI and infrastructure layers may be split and abstracted according to the rules of [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) \(or its subtype [*Separated Presentation*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md#examples--separated-presentation" >}})\)\.
- The system can often be extended with [*Scripts*]({{< relref "../../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}), resulting in a kind of [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}})\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png" alt="Monolith to Layers - Further 2" loading="lazy" width="983" height="461" style="width:100%"/>
</picture>
</a>
</figure>

<nav>

| \<\< [Evolutions of a Monolith that lead to Shards]({{< relref "../../appendices/evolutions/evolutions-of-a-monolith-that-lead-to-shards.md" >}}) | ^ [Evolutions]({{< relref "../../appendices/evolutions/_index.md" >}}) ^ | [Evolutions of a Monolith that make Services]({{< relref "../../appendices/evolutions/evolutions-of-a-monolith-that-make-services.md" >}}) \>\> |
| --- | --- | --- |

</nav>