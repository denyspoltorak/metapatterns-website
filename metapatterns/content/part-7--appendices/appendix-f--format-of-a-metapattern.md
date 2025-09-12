+++
weight = 6
title = "Appendix F. Format of a metapattern."
description = "This section formalized the format of each of the metapattern chapters."
+++

# Appendix F\. Format of a metapattern\.

The descriptions of most metapatterns follow the same format:

### Diagram

The structural diagram \(in *abstractness\-subdomain\-sharding* [coordinates]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}})\) of a typical application of the metapattern\. Please note that in practice the number and types of components and their interactions may vary:

- Even though most diagrams show 3 *layers* or *services*, there are many 2\-layered or 4\-layered systems, while the number of services may often be greater than 10\.
- [*Extension metapatterns*]({{< relref "../part-3--extension-metapatterns/_index.md" >}}) add a *layer* \(or a *layer of services*\) to an existing system, which is shown as [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), but may instead comprise [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}), [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) or even a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}})\.
- Subtypes of [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) or [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) differ in their topologies\. Only one is shown\.
- Components of metapatterns may communicate in various ways that include in\-process calls, RPC, asynchronous messaging or streams\. Only one of them is shown\. Optional communication pathways may appear as dashed arrows\.


Most diagrams feature the following colors:

- *Use cases* \(aka integration, orchestration, workflow or application logic\) are shown in green\. Those are high\-level scenarios executed by user actions or signals from hardware which keep the system acting as a whole\. Use cases are *what* your software does\.
- *Domain logic* \(business rules\), shown in blue, is the set of algorithms that models the real\-world system your software describes\. It is *how* your system solves its tasks\.
- *Generic code* is white\. It stands for tools and libraries unrelated to your business\. Examples include communication protocols, data compression and common maths\.
- *Data* is gray\. It includes business\-critical in\-memory state \(e\.g\. user’s session\) and persistent storage \(in a database or files\)\.


*Use cases* and *domain logic* comprise *business logic* \[[PEAA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#peaa" >}})\] – the code that makes your software different from whatever else is on the market\. It is this part of the system which your customers pay for, and it usually is much larger than the other parts, which makes business logic the primary focus of development\.

In [*choreographed*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) systems use cases are defined by the web of communication channels instead of code inside the system’s components\. That is represented by green arrows and overall lack of green areas on corresponding diagrams\.

### Abstract

*Motto* and the design goal\.

<ins>Known as:</ins> the list of aliases for the general metapattern\.

<ins>Aspects:</ins> an optional list of roles the subject component may have\.

<ins>Variants</ins> or <ins>examples:</ins> one or more lists of notable variations in, patterns or common architectures that derive from or implement the subject metapattern\.

<ins>Structure:</ins> a short description of the structure of the metapattern\.

<ins>Type:</ins> Root, main, extension or implementation\.

- The *root* of all the metapatterns is [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), as any system both looks monolithic to its clients and comes about through division of the continuous \(monolithic\) design space\.
- Main metapatterns \([*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}), [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) and few others derived from them\) stay at the *core* of any architecture\.
- An *extension* adds components to an architecture, built around a main metapattern, to modify its properties\.
- An *implementation* metapattern shows the internal structure of a component which is usually treated as monolithic\.


A short *table of benefits and drawbacks*\.

<ins>References:</ins> select articles and books that describe the topic\.

After that, follow two or three paragraphs of facts and ideas about the metapattern \.

### Performance

This section discusses the performance of the subject metapattern in scenarios which vary in their extent: simple requests or events that relate to a single subsystem are usually processed much faster than those that touch multiple components\.

There are two kinds of performance: latency and throughput\. Low latency is possible only if few components are involved because inter\-component communication, especially in distributed systems, increases latency\. Contrariwise, throughput depends on the number of components that work in parallel, thus it scales together with the system\.

This section may also discuss optimization techniques that apply to the metapattern\.

### Dependencies

Some components of the metapattern depend on other components\. If a component changes, everything that depends on it may need to be re\-tested with the updated version\. If a component’s interface changes, all the components that depend on it must be updated\. Therefore, components that evolve quickly should depend on others, not the other way around\.

Some patterns, like [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}), use [*Adapters*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) to break dependencies\. An *Adapter* depends on components on both sides of itself, making those components independent of each other\. The *Adapters* are small enough to update quickly and may easily be replaced with stubs for testing or running a component in isolation\.

### Applicability

Here follows a list of types of projects which may benefit from applying the architecture under review, and another list of those which it is more likely to hurt\.

### Relations

These are shown through an optional sequence of diagrams, showing the \(*extension* or *implementation*\) metapattern applied to various kinds of architectures, followed by a list of relations between the current and other metapatterns\.

### Variants and examples

A metapattern usually unites many variations of several patterns\. Here we may have a section per dimension of variability and a section for well\-known examples of the pattern\.

On some occasions I had to include several variants that do not properly belong to the metapattern under review, just to avoid confusion with terminology and point the reader to a right chapter\. For example, [*Modular Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md#misapplied-modular-monolith-modulith" >}}) has a module per subdomain, thus it belongs to [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) rather than [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}})\. Still, when the chapter on *Monolith* was not mentioning it, I was blamed for misunderstanding the monolithic architecture\. Such patterns are marked as \(misapplied\) or \(inexact\)\.

I tried to show the difference between synonymous names for every variant or example whenever I could identify one\.

### Evolutions

This covers a brief summary of possible changes to the architecture under review\. Each change leads to a new architecture which usually matches another metapattern\.

[Appendix E]({{< relref "../part-7--appendices/appendix-e--evolutions/_index.md" >}}) discusses many evolutions in greater detail:

- A diagram that shows the original and resulting structure\.
- The list of patterns, present in the resulting architecture\. More general forms of each pattern are given in parentheses, i\.e\. Pattern \(Metapattern \(Parent Metapattern\)\)\.
- The goal\(s\) of the transition\.
- The prerequisites that enable the change\.
- A short description of the change and the resulting system\.
- Lists of pros and cons of the evolution\.
- An optional list of metapatterns that the resulting system may evolve into and their benefits in the context of the current evolution\.


<nav>

| \<\< [Evolutions of a Combined Component]({{< relref "../part-7--appendices/appendix-e--evolutions/evolutions-of-a-combined-component.md" >}}) | ^ [Part 7\. Appendices]({{< relref "../part-7--appendices/_index.md" >}}) ^ | [Appendix G\. Glossary\.]({{< relref "../part-7--appendices/appendix-g--glossary.md" >}}) \>\> |
| --- | --- | --- |

</nav>



