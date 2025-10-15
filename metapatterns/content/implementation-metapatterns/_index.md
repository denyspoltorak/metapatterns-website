+++
weight = 6
title = "Implementation metapatterns"
description = "Implementation patterns study the internals of a component. Plugins, Microkernel and Hexagonal Architecture grant flexibility while Mesh provides resilience."
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Implementation metapatterns {anchor=false}

There are patterns that describe implementation of components:

### [Plugins]({{< relref "../implementation-metapatterns/plugins.md" >}})

<figure>
<a href="/diagrams/Contents/Plugins.png">
<picture>
<source srcset="/diagrams/Contents/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Plugins.png" alt="Plugins" loading="lazy" width="843" height="403" style="width:100%"/>
</picture>
</a>
</figure>

The *Plugins* pattern is about separating a system’s main logic from the customizable details of its behavior\. That allows for the same codebase to be used for multiple flavors or customers\.

*<ins>Includes</ins>*: Plug\-In Architecture, Addons, Strategy, Hooks\.

### [Hexagonal Architecture]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})

<figure>
<a href="/diagrams/Contents/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Contents/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="980" height="363" style="width:100%"/>
</picture>
</a>
</figure>

*Hexagonal Architecture* is a specialization of *Plugins* where every external dependency is isolated behind an *Adapter*, making it easy to update or replace third\-party components\.

*<ins>Includes</ins>*: Ports and Adapters, Onion Architecture, Clean Architecture; Model\-View\-Presenter \(MVP\), Model\-View\-ViewModel \(MVVM\), Model\-View\-Controller \(MVC\), and Action\-Domain\-Responder \(ADR\)\.

### [Microkernel]({{< relref "../implementation-metapatterns/microkernel.md" >}})

<figure>
<a href="/diagrams/Contents/Microkernel.png">
<picture>
<source srcset="/diagrams/Contents/Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Microkernel.png" alt="Microkernel" loading="lazy" width="1031" height="304" style="width:100%"/>
</picture>
</a>
</figure>

This is another derivation of *Plugins*, with a rudimentary *core* component which mediates between resource consumers \(*applications*\) and resource *providers*\. The *Microkernel* is a *Middleware* to the *applications* and an *Orchestrator* to the *providers*\.

*<ins>Includes</ins>*: operating system, software framework, virtualizer, distributed runtime, interpreter, configuration file, Saga Engine, AUTOSAR Classic Platform\.

### [Mesh]({{< relref "../implementation-metapatterns/mesh.md" >}})

<figure>
<a href="/diagrams/Contents/Mesh.png">
<picture>
<source srcset="/diagrams/Contents/Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Mesh.png" alt="Mesh" loading="lazy" width="863" height="261" style="width:100%"/>
</picture>
</a>
</figure>

A *Mesh* consists of intercommunicating shards, each of which may host an application\. The shards coalesce into a fault\-tolerant distributed *Middleware*\.

*<ins>Includes</ins>*: grid; peer\-to\-peer networks, Leaf\-Spine Architecture, Actors, Service Mesh, Space\-Based Architecture\.