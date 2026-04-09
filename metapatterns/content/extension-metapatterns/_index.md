+++
weight = 4
title = "Extension metapatterns"
description = "This part of the book discusses architectural components that integrate systems of services: Middleware, Shared Repository, Proxy, and Orchestrator."
images = ["/diagrams/Web/og/Favicon-plain.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Extension metapatterns {anchor=false}

These patterns extend [*Services*]({{< relref "../basic-metapatterns/services.md" >}}), [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}), or even a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) with a layer that provides an aspect or two of the system’s behavior and often glues other components together\.

### [Middleware]({{< relref "../extension-metapatterns/middleware.md" >}})

<figure>
<a href="/diagrams/Contents/Middleware.png">
<picture>
<source srcset="/diagrams/Contents/Middleware.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Middleware.png" alt="A diagram of Services with a middleware, with explanations." loading="lazy" width="1003" height="263" style="width:100%"/>
</picture>
</a>
</figure>

A [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) is a layer that implements communication between instances of the system’s components and it may also manage the instances\. This way each instance is relieved of the need to track the other instances which it accesses\.

*<ins>Includes</ins>*: \(Message\) Broker and Deployment Manager\.

### [Shared Repository]({{< relref "../extension-metapatterns/shared-repository.md" >}})

<figure>
<a href="/diagrams/Contents/Shared%20Repository.png">
<picture>
<source srcset="/diagrams/Contents/Shared%20Repository.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Shared%20Repository.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Shared%20Repository.png" alt="A diagram of Services with a shared repository, with explanations." loading="lazy" width="963" height="243" style="width:100%"/>
</picture>
</a>
</figure>

A [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) stores the system’s data, maintains its integrity through transactions, and may support subscriptions to changes in subsets of the data\. That lets other system components concentrate on implementing the business logic\.

*<ins>Includes</ins>*: Shared Database, Blackboard, Data Grid of Space\-Based Architecture, Shared Memory, and Shared File System\.

### [Proxy]({{< relref "../extension-metapatterns/proxy.md" >}})

<figure>
<a href="/diagrams/Contents/Proxy.png">
<picture>
<source srcset="/diagrams/Contents/Proxy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Proxy.png" alt="A diagram of Services with a proxy, with explanations." loading="lazy" width="1023" height="363" style="width:100%"/>
</picture>
</a>
</figure>

A [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) mediates between a system and its clients, transparently taking care of some generic functionality\.

*<ins>Includes</ins>*: Full Proxy and Half\-Proxy; Sidecar and Ambassador; Firewall, Response Cache, Load Balancer, Reverse Proxy and various Adapters, e\.g\. Anticorruption Layer, Open Host Service, many Abstraction Layers, Repository and even User Interface\.

### [Orchestrator]({{< relref "../extension-metapatterns/orchestrator.md" >}})

<figure>
<a href="/diagrams/Contents/Orchestrator.png">
<picture>
<source srcset="/diagrams/Contents/Orchestrator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Orchestrator.png" alt="A diagram of Services with an orchestrator, with explanations." loading="lazy" width="1023" height="263" style="width:100%"/>
</picture>
</a>
</figure>

An [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) implements use cases as sequences of calls to the underlying components which are usually left unaware of each other’s existence\.

*<ins>Includes</ins>*: Workflow Owner, Application Layer, Facade, Mediator; API Composer, Scatter\-Gather, MapReduce, Process Manager, Saga Execution Component, and Integration \(Micro\-\)Service\.

### [Sandwich]({{< relref "../extension-metapatterns/sandwich.md" >}})

<figure>
<a href="/diagrams/Contents/Sandwich.png">
<picture>
<source srcset="/diagrams/Contents/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Sandwich.png" alt="A diagram of Sandwich Architecture, with explanations." loading="lazy" width="743" height="243" style="width:93%"/>
</picture>
</a>
</figure>

[*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}) subdivides the largest and loosely coupled [*domain* layer]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) into modules or services while the other layers remain monolithic\.

*<ins>Includes</ins>*: Service\-Based Architecture, Space\-Based Architecture, Blackboard Architecture, Nanoservices, Command Query Responsibility Segregation \(CQRS\)\.