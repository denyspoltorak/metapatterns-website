+++
weight = 10
title = "Sandwich"
description = "In Sandwich, the domain layer is subdivided while the integration and data layers remain monolithic."
images = ["/diagrams/Web/og/Sandwich.png"]
[sitemap]
  priority = 0.8
+++

# Sandwich {anchor=false}

<figure>
<a href="/diagrams/Main/Sandwich.png">
<picture>
<source srcset="/diagrams/Main/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Sandwich.png" alt="Sandwich" loading="lazy" width="1002" height="474" style="width:100%"/>
</picture>
</a>
</figure>

*Follow the line of least resistance\.* Divide where it is loosely coupled\.

<ins>Examples:</ins>

- [Blackboard System](https://en.wikipedia.org/wiki/Blackboard_system) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\],
- Space\-Based Architecture \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\],
- Service\-Based Architecture \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}) [but not]({{< relref "../analytics/ambiguous-patterns.md#service-based-architecture" >}}) [DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\],
- [Command Query Responsibility Segregation](https://martinfowler.com/bliki/CQRS.html) \(CQRS\),
- \(inexact\) Replicated Load\-Balanced Services \[[DDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] / [Lambdas](https://jesseduffield.com/Notes-On-Lambda/)\.


<ins>Structure:</ins> A layer of domain\-level services between shared integration and data layers\.

<ins>Type:</ins> Main, implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Supports medium to large codebases | Aggressive optimization is impossible |
| Multiple specialized development teams | Low fault tolerance |
| There are many ways to evolve the system |  |

<ins>References:</ins> None I know of\.

A *Sandwich* is a \(sub\)system midway between [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\. It emerges when a *layered* component outgrows its architecture and its middle \([*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}})\) layer – which tends to be both the largest and least cohesive – is divided into subdomains while the [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) and [*data*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers remain intact\. Another, less common, origin is a \(sub\)system of [*\(Micro\)Services*]({{< relref "../basic-metapatterns/services.md#part-of-a-subdomain-microservices" >}}) which becomes so tightly coupled that it needs to be partially merged\.

A *Sandwich* includes the following components:

- A shared [*integration layer*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) which receives client requests and dispatches them to the underlying *domain\-level services*\. Though this layer may either implement use cases as an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}), forward each client action to a matching service as a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), or do both as an [*API Gateway*]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}}), it remains the component that ties the entire system together\.
- [*Domain\-level*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) [*services*]({{< relref "../basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}}) \(if distributed\) or [*modules*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) \(otherwise\) that implement the bulk of the system’s business logic and thus contain most of the system’s code\.
- A shared [*data layer*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) which the upper layer’s services operate on\. It may be a database \(provide persistence\) or an in\-memory application state\.


*Sandwiches* often occur naturally without being recognized as a distinct architecture\. In fact, I had to make up a name for this system topology\.

### Performance

As [*domain*\-level]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) services rarely interact among themselves, the performance of a *Sandwich* is similar to that of [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with the same kind of deployment \(components of a *Sandwich* may or may not be colocated\)\.

<figure>
<a href="/diagrams/Performance/Sandwich.png">
<picture>
<source srcset="/diagrams/Performance/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Sandwich.png" alt="Sandwich" loading="lazy" width="1103" height="264" style="width:100%"/>
</picture>
</a>
</figure>

### Dependencies

The *Sandwich*’s [*integration* layer]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) depends on every service inside the *Sandwich*\. The services themselves depend on the [*data* layer]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}})\.

<figure>
<a href="/diagrams/Dependencies/Sandwich.png">
<picture>
<source srcset="/diagrams/Dependencies/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Sandwich.png" alt="Sandwich" loading="lazy" width="863" height="243" style="width:87%"/>
</picture>
</a>
</figure>

Having two shared layers provides three options for invoking the [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) components:

- The most common approach is [*orchestration*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) by the integration layer\.
- [Data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains may rely on [*data change notifications*]({{< relref "../foundations-of-software-architecture/arranging-communication/shared-data.md#full-featured" >}})\.
- [*Choreography*]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) via publish/subscribe is rare because it is inferior to the other two options:
  - It does not help to decouple the subdomain services which remain bound together by the shared layers\.
  - *Publish/subscribe* requires additional libraries and setup, while the datastore, which often supports notifications, is already in place\.
  - Furthermore, *orchestration* allows for much better control over use case logic and error handling than *choreography*\.


### Applicability

*Sandwich* <ins>fits</ins>:

- *Medium\-sized projects with complex domain logic\.* The subdivision of the *domain* layer keeps the code complexity under control and supports development by multiple teams with little increase of operational burden\.
- *Rapid development of ordinary systems\.* If you don’t face any challenging forces, you should keep your architecture [simple and stupid](https://en.wikipedia.org/wiki/KISS_principle) but still flexible – which is the pragmatic *Sandwich*\. You will be able to evolve it in the future if needed\.
- *Data\-centric domains\.* This architecture allows all the services to work on the same dataset, each reading and writing the parts that are involved in its business logic\.


*Sandwich* <ins>does not help</ins>:

- *Projects with a large number of use cases\.* As the [*integration* layer]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) remains monolithic, it still can grow out of control, requiring the system to transform into [*Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}})\.
- *Huge systems\.* Both the [*integration*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) and [*data*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers are cumbersome in that case\.
- *Demanding forces\.* The *Sandwich* architecture is a jack of all trades, master of none\. In general, it is an average backend, and is neither highly elastic, flexible, nor low latency\.


### Relations

<figure>
<a href="/diagrams/Relations/Sandwich.png">
<picture>
<source srcset="/diagrams/Relations/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Sandwich.png" alt="Sandwich" loading="lazy" width="1787" height="523" style="width:100%"/>
</picture>
</a>
</figure>

*Sandwich*:

- Is a system in transition between [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Is [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with a middle layer split into services or [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) sandwiched between layers\.
- May implement a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or *service*\.
- Often provides the internals of a [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#examples--cell" >}})\.
- Includes a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) and an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and/or [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}})\.


<aside>

> A [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#examples--cell" >}}) encapsulates several services so that they can be treated as a single entity with the goal of reducing the number of top\-level system components\. Typically, interdependent and closely communicating services are clustered into a single *Cell*, predisposing the *Cell*’s internals for further merging to reduce the communication overhead and the amount of boilerplate code\. Such a scenario often results in a *Sandwich* trapped inside a *Cell*\.

</aside>

## Examples

Though most real\-world *Sandwiches* stay beneath the radar as non\-standard architectures, there are two well\-documented and highly specialized occurrences of this topology in [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains and a few less stringent generic cases:

### [Blackboard]({{< relref "../extension-metapatterns/shared-repository.md#blackboard" >}}) System

<figure>
<a href="/diagrams/Variants/2/Blackboard.png">
<picture>
<source srcset="/diagrams/Variants/2/Blackboard.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Blackboard.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Blackboard.png" alt="Blackboard" loading="lazy" width="1063" height="326" style="width:100%"/>
</picture>
</a>
</figure>

Some domains are [complex and ill\-structured](http://i.stanford.edu/pub/cstr/reports/cs/tr/86/1123/CS-TR-86-1123.pdf): there is only a vague understanding of how the inputs relate to outputs, therefore you cannot write a single algorithm to solve it\. If you are lucky to have a huge labeled dataset, you can attempt training a neural network\. If there are not so many examples, you are in trouble\.

[*Blackboard Systems*](https://en.wikipedia.org/wiki/Blackboard_system) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] were invented half a century ago to tackle non\-deterministic problems: speech or image recognition, X\-ray crystallography of proteins, or even tracking enemy submarines or stealth aircraft\. In these cases inputs are noisy and scarce and outputs can vary indefinitely, therefore one must go through trial and error proposing, refining, and comparing many possible solutions to arrive at something plausible\.

The system consists of:

- A [*blackboard*]({{< relref "../extension-metapatterns/shared-repository.md#blackboard" >}}) – a [*shared data store*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) that contains all the current knowledge about the problem, namely inputs and hypotheses\. It is subdivided into abstraction levels which range from the original inputs through multiple intermediate representations to the output data structure\. Each level usually contains multiple solution attempts\. 
- *Knowledge sources* – independent, specialized components that process data from a lower level to create a higher\-level hypothesis\. For example, in voice recognition, a knowledge source may read a sound wave and write a letter which that wave encodes\. Another knowledge source may read letters and output English words, while another one tries to find French words\. And the final one collects words into sentences\.
- A [*control*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) – a [*scheduler*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) that assigns processor time to knowledge sources\. It balances the quality and speed of the solution attempts based on the current progress and remaining time\.


This architecture makes best use of every system layer:

- The *control* is required to prune the exponential growth of possible solutions because there are not enough system resources to check all of them\.
- The independent *knowledge sources* are easy to add or remove, allowing the developers to try various algorithms with no changes to other components\.
- The *blackboard* enables the cooperation of knowledge sources, each of which is limited to a small part of the overall solution\.


### [Space\-Based Architecture]({{< relref "../implementation-metapatterns/mesh.md#space-based-architecture" >}})

<figure>
<a href="/diagrams/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.png" alt="Multifunctional - Space-Based Architecture" loading="lazy" width="1283" height="543" style="width:100%"/>
</picture>
</a>
</figure>

*Space\-Based Architecture* \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] is an extremely elastic alternative to [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) which works for [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains\. Each *processing unit* – a [domain\-level]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) service – is co\-located with an in\-memory replica of the entire system’s data, which makes both data access and creation of new instances of *processing units* blazingly fast\. As is common for *Sandwiches*, *processing units* can be [*orchestrated*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) by the [*processing grid*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) or they can [*subscribe to changes*]({{< relref "../foundations-of-software-architecture/arranging-communication/shared-data.md#full-featured" >}}) in the shared [*data grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}), this architecture being flexible enough to allow for choosing a [communication paradigm]({{< relref "../foundations-of-software-architecture/arranging-communication/_index.md" >}}) on per request basis\.

Aside from *processing units*, which contain the main business logic, *Space\-Based Architecture* involves:

- A *messaging grid* which is a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) \(combination of [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository" >}}), [*Dispatcher*]({{< relref "../extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\) that receives, preprocesses, and persists client requests\. Simple requests are forwarded to the least loaded *processing unit* \(service with [domain logic]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}})\) while anything complicated goes to the *processing grid*\.
- A *processing grid* is an optional [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) which manages multi\-step workflows for complicated requests that involve branching and error handling\.
- A [*data grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is a distributed in\-memory database\. It is built of caching [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) nodes which are co\-located with instances of *processing units*, making database access extremely fast\. However, the speed and scalability is paid for with stability – any data in memory is prone to disappearing\. Therefore the *data grid* backs up all the changes to a slower *persistent database*\.
- A *deployment manager* is a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) that creates and destroys instances of *processing units* \(which are paired to the nodes of the *data grid*\), just like [*Service Mesh*]({{< relref "../extension-metapatterns/middleware.md#service-mesh" >}}) does for [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) \(which are paired to [*Sidecars*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}})\)\. However, in contrast to *Service Mesh*, it does not provide a messaging infrastructure because *processing units* communicate by sharing data via the *data grid*, not by sending messages\.


As *Space\-Based Architecture* runs every component in a [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}), it avoids the fault tolerance and database performance drawbacks inherent to *Sandwich*, trading them for possibility of write conflicts when multiple clients cause changes to the same piece of data simultaneously\.

### [Service\-Based Architecture]({{< relref "../basic-metapatterns/services.md#service-based-architecture-sba" >}})

<figure>
<a href="/diagrams/Variants/2/Service-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Variants/2/Service-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Service-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Service-Based%20Architecture.png" alt="Service-Based Architecture" loading="lazy" width="923" height="263" style="width:95%"/>
</picture>
</a>
</figure>

*Service\-Based Architecture* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}) [but not]({{< relref "../analytics/ambiguous-patterns.md#service-based-architecture" >}}) [DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] is the most pragmatic and loosely defined of topologies based on *Services* \(hence the name\)\. In basic *Service\-Based Architecture* [*subdomain services*]({{< relref "../basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}}) are integrated by a [*User Interface*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) layer, usually a *Frontend*, and there is a single [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\. However, as there are no rules for *Service\-Based Architecture*, multiple databases or finer\-grained *GUI*s may be used for programmers’ convenience, disintegrating the *Sandwich* architecture for the sake of less coupled [*Layered Service*s]({{< relref "../fragmented-metapatterns/layered-services.md" >}})\.

<figure>
<a href="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.png" alt="Service-Based to Layered Services" loading="lazy" width="1543" height="286" style="width:100%"/>
</picture>
</a>
</figure>

### [Command Query Responsibility Segregation]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) \(CQRS\)

<figure>
<a href="/diagrams/Variants/2/CQRS.png">
<picture>
<source srcset="/diagrams/Variants/2/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/CQRS.png" alt="CQRS" loading="lazy" width="883" height="263" style="width:84%"/>
</picture>
</a>
</figure>

[*Command Query Responsibility Segregation*](https://martinfowler.com/bliki/CQRS.html) \(*CQRS*\) is a principle which calls for separation of components that process *commands* \(requests that change the system’s data\) and *queries* \(requests that analyze the data\)\. The subdivision necessarily happens at the [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) \(*model*\) level, resulting in separate [*Write Model*](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs) \([*Command Model*](https://martinfowler.com/bliki/CQRS.html)\) and [*Read Model*](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs) \([*Query Model*](https://martinfowler.com/bliki/CQRS.html), [*Thin Read Layer*](https://cqrs.wordpress.com/wp-content/uploads/2010/11/cqrs_documents.pdf)\)\. 

In the simplest case the [*data* layer]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) is kept monolithic, as shown in the diagram above\. That helps decouple the logic\-heavy object\-oriented code which maintains data consistency and business constraints during editing from the search and aggregation code that needs direct database access to run complex SQL queries\. If that is not enough, it is possible to trade complexity for performance by employing specialized databases: [*OLTP*](https://en.wikipedia.org/wiki/Online_transaction_processing) for use with the *Write Model* and [*OLAP*](https://en.wikipedia.org/wiki/Online_analytical_processing) for the *Read Model*, as [described in *Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}):

<figure>
<a href="/diagrams/Variants/2/CQRS%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Variants/2/CQRS%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/CQRS%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/CQRS%20to%20Layered%20Services.png" alt="CQRS to Layered Services" loading="lazy" width="1023" height="323" style="width:100%"/>
</picture>
</a>
</figure>

### \(inexact\) [Replicated Load\-Balanced Services]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-load-balanced-services-work-queue-lambdas" >}}), Lambdas

<figure>
<a href="/diagrams/Variants/1/Shards%20-%20Pool.png">
<picture>
<source srcset="/diagrams/Variants/1/Shards%20-%20Pool.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Shards%20-%20Pool.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Shards%20-%20Pool.png" alt="Shards - Pool" loading="lazy" width="966" height="383" style="width:100%"/>
</picture>
</a>
</figure>

Replicating a system’s [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) [*tier*]({{< relref "../basic-metapatterns/layers.md#distributed-tiers" >}}) along the [*sharding* axis]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}) also results in a *Sandwich*\-like topology, albeit with altered properties: while the original *Sandwich*’s subdivision of the codebase along the [*subdomain* axis]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}) allows for multiteam development and easy integration of new functionality, replication along the *sharding* axis only makes it simple to add or remove instances of the middle tier as the system load changes\.

*Replicated Load\-Balanced Services* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] is the general name for running multiple instances of a stateless service that [*share a database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) and run under a [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\. [*Lambdas*](https://jesseduffield.com/Notes-On-Lambda/) is a synonym for a similar setup from cloud computing providers\.

## Evolutions

The components of a *Sandwich* provide plenty of ways to alter the system, with unique evolutions detailed in [Appendix E]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-sandwich.md" >}}):

### [Primary evolutions]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-sandwich.md" >}})

Some evolutions involve the system’s [domain logic]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) or its topology:

- The *domain\-level services* are independent enough to be easily added or removed\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png" alt="Sandwich add remove Service" loading="lazy" width="1183" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- In most cases they share technologies, allowing for splitting or merging of the services\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png" alt="Sandwich split merge Services" loading="lazy" width="1240" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- If the services are found to be strongly coupled, they can be merged into a monolithic layer, likely to be subdivided in a better way later on\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png" alt="Sandwich to Layers" loading="lazy" width="1104" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- Alternatively, the subdomains can be further decoupled\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png" alt="Sandwich to Layered Services" loading="lazy" width="1343" height="263" style="width:100%"/>
</picture>
</a>
</figure>

### [Evolutions of the data layer]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-shared-repository.md" >}})

If the [*data* layer]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) becomes a performance bottleneck, it can be [split as a *Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md#evolutions" >}}):

- [*Shard*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}}) the database if its records are mutually independent or [*replicate*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) it if it is the read traffic which overloads the system\.
- Apply [*Space\-Based Architecture*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) if elasticity is more important than consistency\.
- Divide the data into databases, private to domain\-level services, transforming the system into [*Orchestrated*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Deploy specialized databases \([*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\)\.


### [Evolutions of the application layer]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-an-orchestrator.md" >}})

If the [*integration* layer]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) contains use cases and becomes cumbersome, it should be subdivided following the [evolutions of *Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md#evolutions" >}}):

- Into [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) if the use cases cluster around subdomains\.
- Into [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) if the system serves several kinds of clients\.
- Into [*Layers*]({{< relref "../extension-metapatterns/orchestrator.md#layered" >}}) if some use cases are simple while others are complicated\.
- Into a [*Hierarchy*]({{< relref "../extension-metapatterns/orchestrator.md#a-service-per-subdomain-hierarchy" >}}) if the use cases include both generic and specialized logic\.


## Summary

*Sandwich* is a pragmatic architecture midway between [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\. It combines simplicity and flexibility, avoiding unnecessary effort for the present while retaining many paths to evolve in the future\.