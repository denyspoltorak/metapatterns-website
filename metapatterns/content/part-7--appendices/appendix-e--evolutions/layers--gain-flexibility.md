+++
weight = 10
title = "Layers: gain flexibility"
+++

# Layers: gain flexibility

The last group of evolutions to consider is about making the system more adaptable\. We have [already discussed]({{< relref "../../part-7--appendices/appendix-e--evolutions/monolith--to-plugins.md" >}}) the following evolutions for [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}):

- The behavior of the system may be modified through [*Plugins*]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}})\.
- [*Hexagonal Architecture*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) protects the business logic from dependencies on libraries and databases\.
- [*Scripts*]({{< relref "../../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) allow for customization of the system’s logic on a per client basis\.


<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png" alt="Monolith to Layers - Further 2" width=100%/>
</p>

</figure>

There is also a new evolution that modifies the upper \(orchestration\) layer:

- The *orchestration layer* may be split into [*Backends for Frontends*]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) to match the needs of several kinds of clients\.


## Divide the orchestration layer into Backends for Frontends

<figure>

<p align="center">
<img src="/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.png" alt="Layers to Backends for Frontends" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}), [Backends for Frontends aka BFFs]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

<ins>Goal</ins>: let each kind of client get a dedicated development team\.

<ins>Prerequisite</ins>: no high\-level logic is shared between client types\.

It is possible that your system has different kinds of users, e\.g\. buyers, sellers, and admins; or web and mobile applications\. It may be easier to support a separate integration module per kind of client than to keep all the unrelated code together in a single integration layer\.

<ins>Pros</ins>:

- Each kind of client gets a dedicated team which may choose best fitting technologies\.
- You get rid of the single large codebase of the integration layer\.


<ins>Cons</ins>: 

- There is no good way to share code between the *BFFs* \(in [naive implementation]({{< relref "../../part-3--extension-metapatterns/orchestrator.md#monolithic" >}})\)\.
- There are new components to administer\.


<ins>Further steps</ins>:

- [Evolve the *BFFs*]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md#evolutions" >}}) through adding a shared *layer* or [*Sidecars*]({{< relref "../../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) for common functionality\.


<nav>

| \<\< [Layers: improve performance]({{< relref "../../part-7--appendices/appendix-e--evolutions/layers--improve-performance.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Services: add or remove services]({{< relref "../../part-7--appendices/appendix-e--evolutions/services--add-or-remove-services.md" >}}) \>\> |
| --- | --- | --- |

</nav>



