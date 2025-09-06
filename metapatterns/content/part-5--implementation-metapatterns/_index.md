+++
weight = 6
title = "Part 5. Implementation Metapatterns"
bookCollapseSection = true
+++

# Part 5\. Implementation Metapatterns

There are patterns that describe implementation of components:

### [Plugins]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})

<figure>

<p align="center">
<img src="/Contents/Plugins.png" alt="Plugins" width=100%/>
</p>

</figure>

The *Plugins* pattern is about separating a system’s main logic from the customizable details of its behavior\. That allows for the same codebase to be used for multiple flavors or customers\.

*<ins>Includes</ins>*: Plug\-In Architecture, Addons, Strategy, Hooks\.

### [Hexagonal Architecture]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}})

<figure>

<p align="center">
<img src="/Contents/Hexagonal Architecture.png" alt="Hexagonal Architecture" width=100%/>
</p>

</figure>

*Hexagonal Architecture* is a specialization of *Plugins* where every external dependency is isolated behind an *Adapter*, making it easy to update or replace third\-party components\.

*<ins>Includes</ins>*: Ports and Adapters, Onion Architecture, Clean Architecture; Model\-View\-Presenter \(MVP\), Model\-View\-ViewModel \(MVVM\), Model\-View\-Controller \(MVC\), and Action\-Domain\-Responder \(ADR\)\.

### [Microkernel]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}})

<figure>

<p align="center">
<img src="/Contents/Microkernel.png" alt="Microkernel" width=100%/>
</p>

</figure>

This is another derivation of *Plugins*, with a rudimentary *core* component which mediates between resource consumers \(*applications*\) and resource *providers*\. The *Microkernel* is a *Middleware* to the *applications* and an *Orchestrator* to the *providers*\.

*<ins>Includes</ins>*: operating system, software framework, virtualizer, distributed runtime, interpreter, configuration file, Saga Engine, AUTOSAR Classic Platform\.

### [Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})

<figure>

<p align="center">
<img src="/Contents/Mesh.png" alt="Mesh" width=100%/>
</p>

</figure>

A *Mesh* consists of intercommunicating shards, each of which may host an application\. The shards coalesce into a fault\-tolerant distributed *Middleware*\.

*<ins>Includes</ins>*: grid; peer\-to\-peer networks, Leaf\-Spine Architecture, Actors, Service Mesh, Space\-Based Architecture\.

<nav>

| \<\< [Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) | ^ [Table of Contents]({{< relref "../_index.md" >}}) ^ | [Plugins]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) \>\> |
| --- | --- | --- |

</nav>



