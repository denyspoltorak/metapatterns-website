+++
weight = 3
title = "Part 2. Basic metapatterns"
description = "Basic architectures include: Monolith, Shards, Layers, Services, and Pipeline. They are building blocks for more complex designs."
bookCollapseSection = true
+++

# Part 2\. Basic metapatterns

Basic metapatterns are both common stand\-alone architectures and building blocks for more complex systems\. They include the single\-component *Monolithic* architecture and the results of its division along each of the [coordinate axes]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}), namely *abstractness*, *subdomain*, and *sharding*:

### [Monolith]({{< relref "../part-2--basic-metapatterns/monolith.md" >}})

<figure style="text-align:center">
<a href="/Contents/Monolith.png" style="outline:none">
<img src="/Contents/Monolith.png" alt="Monolith" style="width:100%"/>
</a>
</figure>

*Monolith* is a single\-component system, the simplest possible architecture\. It is easy to write but hard to evolve and maintain\.

*<ins>Includes</ins>*: Reactor, Proactor, and Half\-Sync/Half\-Async\.

### [Shards]({{< relref "../part-2--basic-metapatterns/shards.md" >}})

<figure style="text-align:center">
<a href="/Contents/Shards.png" style="outline:none">
<img src="/Contents/Shards.png" alt="Shards" style="width:100%"/>
</a>
</figure>

*Shards* are multiple instances of a *Monolith*\. They are scalable but usually require an external component for coordination\.

*<ins>Includes</ins>*: Shards and Amazon Cells, Replicas, Pool of Stateless Instances, and Create on Demand\.

### [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}})

<figure style="text-align:center">
<a href="/Contents/Layers.png" style="outline:none">
<img src="/Contents/Layers.png" alt="Layers" style="width:100%"/>
</a>
</figure>

*Layers* contain one component per level of abstraction\. The layers may vary in technologies and forces and scale individually\.

*<ins>Includes</ins>*: Layers and Tiers\.

### [Services]({{< relref "../part-2--basic-metapatterns/services.md" >}})

<figure style="text-align:center">
<a href="/Contents/Services.png" style="outline:none">
<img src="/Contents/Services.png" alt="Services" style="width:93%"/>
</a>
</figure>

*Services* divide a system into subdomains, often resulting in parts of comparable size assignable to dedicated teams\. However, a system of *Services* is hard to synchronize or debug\.

*<ins>Includes</ins>*: Service\-Based Architecture, Modular Monolith \(Modulith\), Microservices, Device Drivers, and Actors\.

### [Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})

<figure style="text-align:center">
<a href="/Contents/Pipeline.png" style="outline:none">
<img src="/Contents/Pipeline.png" alt="Pipeline" style="width:100%"/>
</a>
</figure>

A *Pipeline* is a kind of *Services* with unidirectional flow\. Each service implements a single step of data processing\. The system is flexible but may grow out of control\. 

*<ins>Includes</ins>*: Pipes and Filters, Choreographed Event\-Driven Architecture, and Data Mesh\.

<nav>

| \<\< [Comparison of communication styles]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}}) | ^ [The pattern language of software architecture]({{< relref "../_index.md" >}}) ^ | [Monolith]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) \>\> |
| --- | --- | --- |

</nav>



