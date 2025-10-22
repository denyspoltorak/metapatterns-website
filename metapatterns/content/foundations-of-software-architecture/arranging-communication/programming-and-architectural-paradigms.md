+++
weight = 1
title = "Programming and architectural paradigms"
description = "The object-oriented, functional, and procedural paradigms are the code-level manifestations of orchestration, choreography, and shared data, respectively."
images = ["/diagrams/Web/og/Favicon-plain.png"]
[sitemap]
  priority = 0.5
+++

# Programming and architectural paradigms {anchor=false}

Sharing a database is the greatest sin when you architect [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}) yet [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) is built around shared data\. How do these approaches coexist? Do *Microservices* make any sense if blatantly violating their rules still results in successful projects?

Another programming paradox holds a clue\. There was C\. Then there came C\+\+ to kill C\. Then we’ve got Rust to kill C\+\+\. Now we have C, C\+\+, and Rust, all of them alive and kickin’\.

## Technologies are specialized

When a new technology emerges, it must show its superiority over existing mature methods\. In most cases that is achieved by specialization\. Is a car superior to a donkey? It depends\. Probably yes, when there are good roads, plenty of gas and spare parts\. A car is narrowly specialized, thus some areas have successfully adopted cars, while others still rely on donkeys\.

The same holds true for programming languages and architectures\. C is good when you work close to hardware and need complete control over whatever happens in the system\. C\+\+ is great at partitioning business logic, but it lost the simplicity of its predecessor\. Rust will likely shine in communication libraries, which are often targeted by hackers, though we have yet to see its wide adoption\. Hence the usefulness \(and choice\) of a tool or programming language depends on the circumstances\.

Let’s turn our attention to your average code\. It often mixes together:

- *Object\-oriented* programming that divides the application into a tree of loosely interacting pieces\.
- *Functional* programming, with the output of one function becoming the input to another, [method chaining](https://en.wikipedia.org/wiki/Method_chaining) included\.
- *Procedural* programming, where multiple functions access the same set of data, which also happens inside classes whose many methods operate their private data members\.


Each [programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) fits its own kind of tasks\. Moreover, the same three approaches reemerge at the system level:

## Object\-oriented \(centralized, shared nothing\) paradigm – orchestration

Almost every software project is too complex for a programmer to keep all the details of its requirements and implementation in their mind\. Notwithstanding, those details must be written down and run as code\.

The good old way out of the trouble is called [*divide and conquer*](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm)\. The global task is divided into several subtasks, and each subtask is subdivided again and again – till the resulting pieces are either simple enough to solve directly or [too messy]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md" >}}) to allow for further subdivision\. Basically, we need to split our domain’s *control*, *logic*, and *data* into a single hierarchy of moderately sized components\.

We have heard a lot about keeping *logic and data* together: an object \(or actor, or module, or service – no matter what you call it\) must own its data to assure its consistency and hide the complexity of the component’s internals from its users\. If the encapsulation of an object's data is violated, the object’s code can neither trust nor restructure it\. On the other hand, if the data is bound to the logic that deals with it, the entire thing becomes a useful black box one does not need to look into to operate\.

Adding *control* to the blend is more subtle, but no less crucial than the encapsulation discussed above\. If an object commands another thing to do something, it must receive the result of the delegated action to know how to proceed with its own task\. Returning control after the action is conducted enables separation of high\-level supervising \(orchestration, integration\) logic from low\-level algorithms which it drives, adding depth to the structure\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Object-oriented.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Object-oriented.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Object-oriented.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Object-oriented.png" alt="Paradigms - Object-oriented" loading="lazy" width="1084" height="543" style="width:100%"/>
</picture>
</a>
</figure>

The ability to address complex domains by reducing the whole to self\-contained pieces makes object\-oriented design ubiquitous\. This paradigm, when applied to distributed systems, gives birth to [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}), [*Orchestrated Services*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), and [*Service\-Oriented Architecture*]({{< relref "../../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Object-oriented%20-%20Variants.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Object-oriented%20-%20Variants.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Object-oriented%20-%20Variants.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Object-oriented%20-%20Variants.png" alt="Paradigms - Object-oriented - Variants" loading="lazy" width="1303" height="483" style="width:100%"/>
</picture>
</a>
</figure>

## Functional \(decentralized, streaming\) paradigm – choreography

Sometimes you don’t need that level of fine\-tuning for the behavior of the system you build – it operates as an [assembly line](https://en.wikipedia.org/wiki/Assembly_line) with high throughput and little variance: its logic is made of steps that resemble work stations along a [conveyor belt](https://en.wikipedia.org/wiki/Conveyor_belt) through which identically structured pieces of data flow, just like goods on the belt\. In that case there is very little to control: if an item is good, it goes further, otherwise it just falls off the line\. Here the *control* resides in the graph of connections*,* the domain *logic* is subdivided, while the *data* is copied between the components\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Functional.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Functional.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Functional.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Functional.png" alt="Paradigms - Functional" loading="lazy" width="994" height="362" style="width:100%"/>
</picture>
</a>
</figure>

Functional or pipelined design is famous for its simplicity and high performance as the majority of processing steps can be scaled\. However, its straightforward application lacks the depth needed for handling complex processes, which would translate into webs of relations between hundreds of functions present at the same level of design\. It is also inefficient for choose\-your\-own\-adventure\-style \([*control*]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\) systems where too many too short conveyor belts would be required, negating the paradigm’s benefits\. And it may not be the right tool for making small changes in large sets of data as you’ll likely need to copy the whole dataset between the constituent functions\.

In distributed systems the functional paradigm is disguised as [*Choreographed Event\-Driven Architecture*]({{< relref "../../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}), [*Data Mesh*]({{< relref "../../basic-metapatterns/pipeline.md#data-mesh" >}}), and various [batch or stream]({{< relref "../../basic-metapatterns/pipeline.md#variants-by-scheduling" >}}) processing \[[DDIA]({{< relref "../../appendices/books-referenced.md#ddia" >}})\] [*Pipelines*]({{< relref "../../basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}})\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Functional%20-%20Variants.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Functional%20-%20Variants.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Functional%20-%20Variants.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Functional%20-%20Variants.png" alt="Paradigms - Functional - Variants" loading="lazy" width="1124" height="345" style="width:100%"/>
</picture>
</a>
</figure>

## Procedural \(data\-centric\) paradigm – shared data

The final approach is integration through data\. There are cases where the domain data and business logic differ in structure – you cannot divide your project into objects because each of the many pieces of its logic needs to access several \(seemingly unrelated\) parts of its data\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Data-centric.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Data-centric.png" alt="Paradigms - Data-centric" loading="lazy" width="944" height="501" style="width:100%"/>
</picture>
</a>
</figure>

In the data\-centric paradigm *logic* and *data* are structured independently\. In procedural programming, like in object\-oriented paradigm, *control* is implemented inside the logic, making the logic layer hierarchical \(*orchestrated*\)\. Another, much less common, option relies on *Observer* \[[GoF]({{< relref "../../appendices/books-referenced.md#gof" >}})\] to provide data change notifications, resulting in decentralized \(*choreographed*\) application logic:

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Notifications.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Notifications.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Notifications.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Notifications.png" alt="Paradigms - Data-centric - Notifications" loading="lazy" width="944" height="463" style="width:100%"/>
</picture>
</a>
</figure>

The data\-centric approach works well for moderately\-sized projects with a stable data model \(like reservation of seats in trains or game of chess\)\. The best\-known distributed data\-centric architectures include [*Services with a Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) and [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.

<figure>
<a href="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Variants.png">
<picture>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Variants.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Variants.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Paradigms%20-%20Data-centric%20-%20Variants.png" alt="Paradigms - Data-centric - Variants" loading="lazy" width="943" height="463" style="width:100%"/>
</picture>
</a>
</figure>

## Composite cases

The three programming paradigms tend to collaborate:

- An ordinary class is object\-oriented on the outside but procedural inside: each of its methods can access any of its private data members\. Moreover, code inside methods may chain function calls, locally applying the functional paradigm\.
- [*Cell\-Based Architecture*]({{< relref "../../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) tends to use *choreography* \(pub/sub\) between *Cells* \[[DEDS]({{< relref "../../appendices/books-referenced.md#deds" >}})\] and *orchestration* or communication via a *shared database* inside them\.
- A system of [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) \(or [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}})\) may be integrated through both [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) and [*Shared Database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \(or *processing grid* and [*data grid*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}), correspondingly\)\.


## Reality is more complex

We have reviewed a few cases directly supported by common programming languages\. However, there is a wide variety of possible combinations of \(at least\) the following dimensions, each making a unique programming paradigm:

- Synchronous \(method calls\) vs asynchronous \(messaging\), with closely related:
  - Imperative vs reactive\.
  - Blocking vs non\-blocking\.
- Centralized \(orchestrated\) vs decentralized \(choreographed\) flow\.
- Shared data \(tuple space\) vs [shared nothing](https://en.wikipedia.org/wiki/Shared-nothing_architecture) \(messaging\)\.
- Commands \(actors\) vs notifications \(agents\)\.
- One\-to\-one \(channels\) vs many\-to\-one \(mailboxes\) vs one\-to\-many \(multicast\) vs many\-to\-many \(gossip\) communication\.


Some of the combinations look impossible or impractical, others are narrowly specialized thus uncommon, while many more are commonplace\. Discussing all of them would require insights from people who have used them in practice and that would take a dedicated book\.

## Summary

We have deconstructed the most common programming paradigms into their driving forces and shown how those forces shape distributed architectures:

- An object\-oriented system relies on hierarchical decomposition of a complex domain, just like *SOA* and *Orchestrated \(Micro\-\)Services* do\.
- Functional programming streams data through a sequence of transformations, which is the idea behind *Choreographed Event\-Driven Architecture* and *Data Mesh*\.
- Procedural style lets any piece of logic access the entire project’s data, resembling *Space\-Based Architecture* and *Services with a Shared Database*\.


Now let’s examine each of these approaches in depth: