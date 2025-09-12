+++
weight = 4
title = "Part 3. Extension metapatterns"
description = "A system of services can be extended with one or more specialized layers such as a Proxy, a Middleware, an Orchestrator, or a Shared Repository."
bookCollapseSection = true
+++

# Part 3\. Extension metapatterns

These patterns extend *Services*, *Shards*, or even a *Monolith* with a layer that provides an aspect or two of the system’s behavior and often glues other components together\.

### [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})

<figure style="text-align:center">
<a href="/Contents/Middleware.png" style="outline:none">
<img src="/Contents/Middleware.png" alt="Middleware" style="width:100%"/>
</a>
</figure>

*Middleware* is a layer that implements communication between instances of the system’s components and it may also manage the instances\. This way each instance is relieved of the need to track the other instances which it accesses\.

*<ins>Includes</ins>*: \(Message\) Broker and Deployment Manager\.

### [Shared Repository]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})

<figure style="text-align:center">
<a href="/Contents/Shared%20Repository.png" style="outline:none">
<img src="/Contents/Shared%20Repository.png" alt="Shared Repository" style="width:100%"/>
</a>
</figure>

A *Shared Repository* stores the system’s data, maintains its integrity through transactions, and may support subscriptions to changes in subsets of the data\. That lets other system components concentrate on implementing the business logic\.

*<ins>Includes</ins>*: Shared Database, Blackboard, Data Grid of Space\-Based Architecture, Shared Memory, and Shared File System\.

### [Proxy]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})

<figure style="text-align:center">
<a href="/Contents/Proxy.png" style="outline:none">
<img src="/Contents/Proxy.png" alt="Proxy" style="width:100%"/>
</a>
</figure>

A *Proxy* mediates between a system and its clients, transparently taking care of some generic functionality\.

*<ins>Includes</ins>*: Full Proxy and Half\-Proxy; Sidecar and Ambassador; Firewall, Response Cache, Load Balancer, Reverse Proxy and various Adapters, e\.g\. Anticorruption Layer, Open Host Service, XXX Abstraction Layers and Repository\.

### [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})

<figure style="text-align:center">
<a href="/Contents/Orchestrator.png" style="outline:none">
<img src="/Contents/Orchestrator.png" alt="Orchestrator" style="width:100%"/>
</a>
</figure>

An *Orchestrator* implements use cases as sequences of calls to the underlying components which are usually left unaware of each other’s existence\.

*<ins>Includes</ins>*: Workflow Owner, Application Layer, Facade, Mediator; API Composer, Scatter\-Gather, MapReduce, Process Manager, Saga Execution Component, and Integration \(Micro\-\)Service\.

### [Combined Component]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}})

Several patterns combine the functionality of two or more extension layers\.

*<ins>Includes</ins>*: Message Bus, API Gateway, Event Mediator, Enterprise Service Bus, Service Mesh, Middleware of Space\-Based Architecture, and Shared Event Store\.

<nav>

| \<\< [Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) | ^ [The pattern language of software architecture]({{< relref "../_index.md" >}}) ^ | [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) \>\> |
| --- | --- | --- |

</nav>



