+++
weight = 3
title = "Basic metapatterns"
description = "Basic architectures include: Monolith, Shards, Layers, Services, and Pipeline. They are the building blocks for more complex designs."
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Basic metapatterns

Basic metapatterns are both common stand\-alone architectures and building blocks for more complex systems\. They include the single\-component *Monolithic* architecture and the results of its division along each of the [coordinate axes]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}), namely *abstractness*, *subdomain*, and *sharding*:

### [Monolith]({{< relref "../basic-metapatterns/monolith.md" >}})

<figure>
<a href="/diagrams/Contents/Monolith.png">
<img src="/diagrams/Contents/Monolith.svg" alt="Monolith" loading="lazy" width="1003" height="243" style="width:100%"/>
</a>
</figure>

*Monolith* is a single\-component system, the simplest possible architecture\. It is easy to write but hard to evolve and maintain\.

*<ins>Includes</ins>*: Reactor, Proactor, and Half\-Sync/Half\-Async\.

### [Shards]({{< relref "../basic-metapatterns/shards.md" >}})

<figure>
<a href="/diagrams/Contents/Shards.png">
<img src="/diagrams/Contents/Shards.svg" alt="Shards" loading="lazy" width="783" height="243" style="width:100%"/>
</a>
</figure>

*Shards* are multiple instances of a *Monolith*\. They are scalable but usually require an external component for coordination\.

*<ins>Includes</ins>*: Shards and Amazon Cells, Replicas, Pool of Stateless Instances, and Create on Demand\.

### [Layers]({{< relref "../basic-metapatterns/layers.md" >}})

<figure>
<a href="/diagrams/Contents/Layers.png">
<img src="/diagrams/Contents/Layers.svg" alt="Layers" loading="lazy" width="783" height="243" style="width:100%"/>
</a>
</figure>

*Layers* contain one component per level of abstraction\. The layers may vary in technologies and forces and scale individually\.

*<ins>Includes</ins>*: Layers and Tiers\.

### [Services]({{< relref "../basic-metapatterns/services.md" >}})

<figure>
<a href="/diagrams/Contents/Services.png">
<img src="/diagrams/Contents/Services.svg" alt="Services" loading="lazy" width="943" height="246" style="width:93%"/>
</a>
</figure>

*Services* divide a system into subdomains, often resulting in parts of comparable size assignable to dedicated teams\. However, a system of *Services* is hard to synchronize or debug\.

*<ins>Includes</ins>*: Service\-Based Architecture, Modular Monolith \(Modulith\), Microservices, Device Drivers, and Actors\.

### [Pipeline]({{< relref "../basic-metapatterns/pipeline.md" >}})

<figure>
<a href="/diagrams/Contents/Pipeline.png">
<img src="/diagrams/Contents/Pipeline.svg" alt="Pipeline" loading="lazy" width="1003" height="203" style="width:100%"/>
</a>
</figure>

A *Pipeline* is a kind of *Services* with unidirectional flow\. Each service implements a single step of data processing\. The system is flexible but may grow out of control\. 

*<ins>Includes</ins>*: Pipes and Filters, Choreographed Event\-Driven Architecture, and Data Mesh\.

<nav>

| \<\< [Comparison of communication styles]({{< relref "../foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}}) | ^ [The pattern language of software architecture]({{< relref "../_index.md" >}}) ^ | [Monolith]({{< relref "../basic-metapatterns/monolith.md" >}}) \>\> |
| --- | --- | --- |

</nav>