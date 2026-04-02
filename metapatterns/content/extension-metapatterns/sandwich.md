+++
weight = 10
title = "Sandwich"
description = "This chapter explores sandwiched architectures: Blackboard System, Space-Based Architecture, Service-Based Architecture, CQRS, Nanoservices, and Lambdas."
images = ["/diagrams/Web/og/Sandwich.png"]
primary_image = "/diagrams/Main/Sandwich.png"
[sitemap]
  priority = 0.8
+++

# Sandwich {anchor=false}

<figure>
<a href="/diagrams/Main/Sandwich.png">
<picture>
<source srcset="/diagrams/Main/Sandwich.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Sandwich.png" alt="Sandwich Architecture, with a legend." loading="lazy" width="1002" height="474" style="width:100%"/>
</picture>
</a>
</figure>

*Follow the line of least resistance\.* Divide where it is loosely coupled\.

<ins>Structure:</ins> A layer of domain\-level services between shared integration and data layers\.

<ins>Type:</ins> System topology, implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Supports medium to large codebases | Aggressive optimization is impossible |
| Multiple specialized development teams | Low fault tolerance |
| There are many ways to evolve the system |  |

<ins>References:</ins> None I know of\.

A *Sandwich* is a \(sub\)system midway between [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\. It emerges when a *layered* component outgrows its architecture and its middle \([*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}})\) layer – which tends to be both the largest and least cohesive – is divided into subdomains while the [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) and [*data*]({{< relref "../basic-metapatterns/layers.md#data-persistence" >}}) layers remain intact\. Another, less common, origin is a \(sub\)system of [*\(Micro\)Services*]({{< relref "../basic-metapatterns/services.md#part-of-a-subdomain-microservices" >}}) which becomes so tightly coupled that it needs to be partially merged\.

A *Sandwich*, named after the shape of its diagram, includes the following components:

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
<img src="/diagrams/Performance/Sandwich.png" alt="Control and data flow is identical in Sandwich and Layers." loading="lazy" width="1103" height="264" style="width:100%"/>
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
<img src="/diagrams/Dependencies/Sandwich.png" alt="The integration layer depends on every service. Every service depends on the data layer." loading="lazy" width="863" height="243" style="width:87%"/>
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
<img src="/diagrams/Relations/Sandwich.png" alt="Transitions between Layers, a Sandwich, a Service-Based Architecture, and Layered Services." loading="lazy" width="1787" height="523" style="width:100%"/>
</picture>
</a>
</figure>

*Sandwich*:

- Is a system in transition between [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) and [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Is [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with a middle layer split into services or [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) sandwiched between layers\.
- May implement a [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or *service*\.
- Often provides the internals of a [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\.
- Includes a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) and an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and/or [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}})\.


<aside>

> A [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}}) encapsulates several services so that they can be treated as a single entity with the goal of reducing the number of top\-level system components\. Typically, interdependent and closely communicating services are clustered into a single *Cell*, predisposing the *Cell*’s internals for further merging to reduce the communication overhead and the amount of boilerplate code\. Such a scenario often results in a *Sandwich* trapped inside a *Cell*\.

</aside>

## Examples

Though most real\-world *Sandwiches* stay beneath the radar as non\-standard architectures, there are two well\-documented and highly specialized occurrences of this topology in [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains and a few less stringent generic cases:

- [*Blackboard System*]({{< relref "#blackboard-system" >}}) describes how specialized algorithms cooperate to try solving a non\-deterministic problem\.
- [*Space\-Based Architecture*]({{< relref "#space-based-architecture" >}}) runs multiple instances of services co\-located with a distributed in\-memory database\.
- [*Service\-Based Architecture*]({{< relref "#service-based-architecture" >}}) comprises large services that often share a database\.
- [*Nanoservices*]({{< relref "#nanoservices" >}}) is a system of functions deployed to a cloud\.
- [*Command Query Responsibility Segregation*]({{< relref "#command-query-responsibility-segregation-cqrs" >}}) uses different models for read\-write and read\-only scenarios\. 
- [*Replicated Load\-Balanced Services*]({{< relref "#inexact-replicated-load-balanced-services-lambdas" >}}) are identical instances of a stateless service that access a shared database\.


### [Blackboard]({{< relref "../extension-metapatterns/shared-repository.md#blackboard" >}}) System

<figure>
<a href="/diagrams/Variants/2/Blackboard.png">
<picture>
<source srcset="/diagrams/Variants/2/Blackboard.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Blackboard.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Blackboard.png" alt="A Blackboard System includes a control which orchestrates knowledge sources which access a blackboard with shared data." loading="lazy" width="1063" height="326" style="width:100%"/>
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
<img src="/diagrams/Variants/2/Multifunctional%20-%20Space-Based%20Architecture.png" alt="Space-Based Architecture comprises the following layers: a messaging grid, a processing grid, scaled processing units, a data grid, a deployment manager, and a persistent database." loading="lazy" width="1283" height="543" style="width:100%"/>
</picture>
</a>
</figure>

*Space\-Based Architecture* \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] is an extremely elastic alternative to [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) which works for [data\-centric]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) domains\. Each *processing unit* – a [domain\-level]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) service – is co\-located with an in\-memory replica of the entire system’s data, which makes both data access and creation of new instances of *processing units* blazingly fast\. As is common for *Sandwiches*, *processing units* can be [*orchestrated*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) by the [*processing grid*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) or they can [*subscribe to changes*]({{< relref "../foundations-of-software-architecture/arranging-communication/shared-data.md#full-featured" >}}) in the shared [*data grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}), this architecture being flexible enough to allow for choosing a [communication paradigm]({{< relref "../foundations-of-software-architecture/arranging-communication/_index.md" >}}) on per request basis\.

Aside from *processing units*, which contain the main business logic, *Space\-Based Architecture* involves:

- A *messaging grid* which is a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) \(combination of [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}), [*Dispatcher*]({{< relref "../extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\) that receives, preprocesses, and persists client requests\. Simple requests are forwarded to the least loaded *processing unit* \(service with [domain logic]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}})\) while anything complicated goes to the *processing grid*\.
- A *processing grid* is an optional [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) which manages multi\-step workflows for complicated requests that involve branching and error handling\.
- A [*data grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) is a distributed in\-memory database\. It is built of caching [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) nodes which are co\-located with instances of *processing units*, making database access extremely fast\. However, the speed and scalability is paid for with stability – any data in memory is prone to disappearing\. Therefore the *data grid* backs up all the changes to a slower *persistent database*\.
- A *deployment manager* is a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) that creates and destroys instances of *processing units* \(which are paired to the nodes of the *data grid*\), just like [*Service Mesh*]({{< relref "../extension-metapatterns/middleware.md#service-mesh" >}}) does for [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) \(which are paired to [*Sidecars*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}})\)\. However, in contrast to *Service Mesh*, it does not provide a messaging infrastructure because *processing units* communicate by sharing data via the *data grid*, not by sending messages\.


As *Space\-Based Architecture* runs every component in a [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}), it avoids the fault tolerance and database performance drawbacks inherent to *Sandwich*, trading them for possibility of write conflicts when multiple clients cause changes to the same piece of data simultaneously\.

### [Service\-Based Architecture]({{< relref "../basic-metapatterns/services.md#service-based-architecture-sba-macroservices" >}})

<figure>
<a href="/diagrams/Variants/2/Service-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Variants/2/Service-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Service-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Service-Based%20Architecture.png" alt="A Sandwich-like topology with user interface, a layer of domain services, and a shared database." loading="lazy" width="923" height="263" style="width:95%"/>
</picture>
</a>
</figure>

*Service\-Based Architecture* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}) [but not]({{< relref "../analytics/ambiguous-patterns.md#service-based-architecture" >}}) [DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] is the most pragmatic and loosely defined of topologies based on *Services* \(hence the name\)\. In basic *Service\-Based Architecture* [*subdomain services*]({{< relref "../basic-metapatterns/services.md#whole-subdomain-sub-domain-services-macroservices" >}}) are integrated by a [*User Interface*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) layer, usually a *Frontend*, and there is a single [*Shared Database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\. However, as there are no rules for *Service\-Based Architecture*, multiple databases or finer\-grained *GUI*s may be used for programmers’ convenience, disintegrating the *Sandwich* architecture for the sake of less coupled [*Layered Service*s]({{< relref "../fragmented-metapatterns/layered-services.md" >}})\.

<figure>
<a href="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Service-Based%20to%20Layered%20Services.png" alt="A Sandwich-like topology with shared user interface and database is gradually transformed into layered services." loading="lazy" width="1543" height="286" style="width:100%"/>
</picture>
</a>
</figure>

### [Nanoservices]({{< relref "../basic-metapatterns/services.md#inexact-nanoservices-api-layer" >}})

<figure>
<a href="/diagrams/Variants/2/Nanoservices.png">
<picture>
<source srcset="/diagrams/Variants/2/Nanoservices.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/Nanoservices.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/Nanoservices.png" alt="Nanoservices form a Sandwich-shaped architecture. The upper layer is an API Gateway for an orchestrated system or a gateway for pipelined Nanoservices. The lower layer is a shared datastore." loading="lazy" width="1243" height="363" style="width:100%"/>
</picture>
</a>
</figure>

*Nanoservices* is a loosely defined architecture built of *functions* \([FaaS](https://en.wikipedia.org/wiki/Function_as_a_service)\) individually deployed to the cloud\. It is highly elastic and relies on a cloud provider for operational support, saving costs for small businesses\.

As a single *nanoservice* is too small to implement a use case, there must be an [*integration layer*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) that receives client or user requests, runs several *nasoservices* to execute it, and returns a response\. The integration layer may be a [*Frontend*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}), [*API Gateway*]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}}), or an [*Event Mediator*]({{< relref "../extension-metapatterns/orchestrator.md#event-mediator" >}}) for [*orchestrated Nanoservices*]({{< relref "../basic-metapatterns/services.md#inexact-nanoservices-api-layer" >}}) or an ordinary [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) that initiates a [*pipelined*]({{< relref "../basic-metapatterns/pipeline.md" >}}) scenario and receives its response from a [*choreographed* system]({{< relref "../basic-metapatterns/pipeline.md#function-as-a-service-faas-nanoservices-pipelined" >}})\.

As each *nanoservice* is stateless, for the system to be stateful there must be a *datastore*\. And as each *nanoservice* can do one thing \(read, write, or search\), the *datastore* must be [*shared*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) to be of any use\.

This example shows how a fine\-grained decomposition of business logic necessarily results in a *Sandwich* architecture\.

### [Command Query Responsibility Segregation]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) \(CQRS\)

<figure>
<a href="/diagrams/Variants/2/CQRS.png">
<picture>
<source srcset="/diagrams/Variants/2/CQRS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/2/CQRS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/2/CQRS.png" alt="A large read and smaller write models between a user interface and database." loading="lazy" width="883" height="263" style="width:84%"/>
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
<img src="/diagrams/Variants/2/CQRS%20to%20Layered%20Services.png" alt="The single database of a Sandwich-like CQRS with a shared database is subdivided into OLTP and OLAP databases, forming Layered Services." loading="lazy" width="1023" height="323" style="width:100%"/>
</picture>
</a>
</figure>

### \(inexact\) [Replicated Load\-Balanced Services]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-load-balanced-services-work-queue-lambdas" >}}), Lambdas

<figure>
<a href="/diagrams/Variants/1/Shards%20-%20Pool.png">
<picture>
<source srcset="/diagrams/Variants/1/Shards%20-%20Pool.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Shards%20-%20Pool.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Shards%20-%20Pool.png" alt="A load balancer connects a new client to a free instance of a stateless backend that accesses a database shared among all the backend instances." loading="lazy" width="966" height="383" style="width:100%"/>
</picture>
</a>
</figure>

Replicating a system’s [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) [*tier*]({{< relref "../basic-metapatterns/layers.md#distributed-tiers" >}}) along the [*sharding* axis]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}) also results in a *Sandwich*\-like topology, albeit with altered properties: while the original *Sandwich*’s subdivision of the codebase along the [*subdomain* axis]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}) allows for multiteam development and easy integration of new functionality, replication along the *sharding* axis only makes it simple to add or remove instances of the middle tier as the system load changes\.

*Replicated Load\-Balanced Services* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] is the general name for running multiple instances of a stateless service that [*share a database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) and run under a [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\. [*Lambdas*](https://jesseduffield.com/Notes-On-Lambda/) is a synonym for a similar setup from cloud computing providers \(see [*Nanoservices*]({{< relref "#nanoservices" >}})\)\.

## Evolutions

The components of a *Sandwich* provide plenty of ways to alter the system, with unique evolutions detailed in [Appendix E]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-sandwich.md" >}}):

### [Primary evolutions]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-sandwich.md" >}})

[Some evolutions]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-a-sandwich.md" >}}) involve the system’s [domain logic]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) or its topology:

- The *domain\-level services* are independent enough to be easily added or removed\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png" alt="One of the domain-level services is removed and another one is added." loading="lazy" width="1183" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- In most cases they share technologies, allowing for splitting or merging of the services\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png" alt="One domain-level service is split in half while two other services are merged together." loading="lazy" width="1240" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- If the services are found to be strongly coupled, they can be merged into a monolithic layer, likely to be subdivided in a better way later on\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png" alt="The entire domain layer is merged, resulting in Layers." loading="lazy" width="1104" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- Alternatively, the subdomains can be further decoupled\.


<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png" alt="The integration and data layers are divided into subdomains, producing Three-Layered Services." loading="lazy" width="1343" height="263" style="width:100%"/>
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