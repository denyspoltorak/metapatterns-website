+++
weight = 10
title = "Evolutions of Layers to gain flexibility"
description = "The upper Proxy or Orchestrator layer can be split into a service per client, making Backends for Frontends. This allows for per-client customization."
images = ["/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.svg"]
[sitemap]
  priority = 0.3
+++

# Evolutions of Layers to gain flexibility

The last group of evolutions to consider is about making the system more adaptable\. We have [already discussed]({{< relref "../../appendices/evolutions/evolutions-of-a-monolith-that-rely-on-plugins.md" >}}) the following evolutions for [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}):

- The behavior of the system may be modified through [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}})\.
- [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) protects the business logic from dependencies on libraries and databases\.
- [*Scripts*]({{< relref "../../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) allow for customization of the system’s logic on a per client basis\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png">
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.svg" alt="Monolith to Layers - Further 2" loading="lazy" width="983" height="461" style="width:100%"/>
</a>
</figure>

There is also a new evolution that modifies the upper \(orchestration\) layer:

- The *orchestration layer* may be split into [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) to match the needs of several kinds of clients\.


## Divide the orchestration layer into Backends for Frontends

<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.png">
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.svg" alt="Layers to Backends for Frontends" loading="lazy" width="1003" height="323" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}}), [Backends for Frontends aka BFFs]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

<ins>Goal</ins>: let each kind of client get a dedicated development team\.

<ins>Prerequisite</ins>: no high\-level logic is shared between client types\.

It is possible that your system has different kinds of users, e\.g\. buyers, sellers, and admins; or web and mobile applications\. It may be easier to support a separate integration module per kind of client than to keep all the unrelated code together in a single integration layer\.

<ins>Pros</ins>:

- Each kind of client gets a dedicated team which may choose best fitting technologies\.
- You get rid of the single large codebase of the integration layer\.


<ins>Cons</ins>: 

- There is no good way to share code between the *BFFs* \(in [naive implementation]({{< relref "../../extension-metapatterns/orchestrator.md#monolithic" >}})\)\.
- There are new components to administer\.


<ins>Further steps</ins>:

- [Evolve the *BFFs*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md#evolutions" >}}) through adding a shared *layer* or [*Sidecars*]({{< relref "../../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) for common functionality\.


<nav>

| \<\< [Evolutions of Layers to improve performance]({{< relref "../../appendices/evolutions/evolutions-of-layers-to-improve-performance.md" >}}) | ^ [Evolutions]({{< relref "../../appendices/evolutions/_index.md" >}}) ^ | [Evolutions of Services that add or remove services]({{< relref "../../appendices/evolutions/evolutions-of-services-that-add-or-remove-services.md" >}}) \>\> |
| --- | --- | --- |

</nav>