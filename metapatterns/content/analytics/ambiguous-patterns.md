+++
weight = 2
title = "Ambiguous patterns"
description = "There are patterns with meanings which vary from source to source. They include Monolith, Reactor, Microkernel, Domain Services, Cells, and Nanoservices."
images = ["/diagrams/Web/og/Ambiguous.png"]
[sitemap]
  priority = 0.5
+++

# Ambiguous patterns {anchor=false}

We’ve seen a single pattern come under many names, as happens with [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}), and also one name used for multiple topologies, as with [*Services*]({{< relref "../basic-metapatterns/services.md" >}}), which may [orchestrate each other]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}}), make a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}), or be components of the [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) \(*SOA*\)\. On top of that, there are several pattern names that are often believed to be unambiguous while each of them sees conflicting definitions in books or over the web \(thanks to [*Semantic Diffusion*](https://martinfowler.com/bliki/SemanticDiffusion.html) or independent coining of the term by multiple authors\)\. Let’s explore the last kind, which is the most dangerous both for your understanding of other people and for your time wasted in arguments\.

### Monolith

<figure>
<a href="/diagrams/Conclusion/Ambiguous-Monolith.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-Monolith.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-Monolith.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-Monolith.png" alt="Ambiguous-Monolith" loading="lazy" width="1083" height="763" style="width:100%"/>
</picture>
</a>
</figure>

The old books, namely \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] and \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\], described a *tightly coupled* [*unstructured* system]({{< relref "../basic-metapatterns/monolith.md" >}}), where anything depends on everything, as *monolithic*, which matched the meaning of the word in Latin – “single stone”\.

Then something evil happened – I believe that the proponents of [*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), backed by the hype and money they had earned from corporations, started labeling any *non\-distributed* system as *monolithic*, obviously to contrast the negative connotation of the word to their own most progressive design\.

It took only a decade for the karma to strike back – when the new generation behind [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) redefined *monolithic* as a *single unit of deployment* – to call the now obsolescent *SOA* systems [*Distributed Monoliths*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#distributed-monolith" >}}) \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] because their services used to grow so coupled that they had to be deployed together\.

The novel misnomers, [*Layered Monolith*]({{< relref "../basic-metapatterns/layers.md#synchronous-layers-layered-monolith" >}}) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] and [*Modular Monolith*]({{< relref "../basic-metapatterns/services.md#asynchronous-modules-modular-monolith-modulith-embedded-actors" >}}) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], which denote an application partitioned by abstractness or subdomain, correspondingly, add to the confusion\.

### Reactor

<figure>
<a href="/diagrams/Variants/1/Subtypes%20of%20Monolith.png">
<picture>
<source srcset="/diagrams/Variants/1/Subtypes%20of%20Monolith.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Subtypes%20of%20Monolith.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Subtypes%20of%20Monolith.png" alt="Subtypes of Monolith" loading="lazy" width="848" height="366" style="width:100%"/>
</picture>
</a>
</figure>

People [tend to call](http://ithare.com/category/reactors/) any event\-driven service a *Reactor*\. In fact, there are three patterns that describe threading models for an event\-handling system:

- A [*Reactor*]({{< relref "../basic-metapatterns/monolith.md#multi-threaded-reactor-a-thread-per-task" >}}) \[[POSA2]({{< relref "../appendices/books-referenced.md#posa2" >}})\] runs each task in a dedicated thread and blocks it on any calls outside of the component\. That allows for normal *imperative programming* but is resource\-consuming and not very responsive or flexible\.
- A [*Proactor*]({{< relref "../basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) \[[POSA2]({{< relref "../appendices/books-referenced.md#posa2" >}})\] relies on a single thread to serve all the system’s tasks in an interleaved manner, just like an OS uses a CPU core to run multiple processes\. The resulting non\-blocking code is fragmented \(thus known as *callback hell*\) but it can address any incoming event immediately\. This suits [real\-time control systems]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\.
- [*Half\-Sync/Half\-Async*]({{< relref "../basic-metapatterns/monolith.md#inexact-half-synchalf-async-coroutines-or-fibers" >}}) \[[POSA2]({{< relref "../appendices/books-referenced.md#posa2" >}})\] is what we know better as *coroutines* or *fibers* – there are multiple *Reactor*\-like lightweight threads that block on a *Proactor*\-like engine which translates between synchronous calls from the user code and asynchronous system events\.


In most cases we’ll hear of *Proactor* being called *Reactor* – probably because *Reactor* was historically the first and the simplest of the three patterns, and it is similar in name to *reactive programming* found in *Proactor*\.

### Microkernel

<figure>
<a href="/diagrams/Conclusion/Ambiguous-Microkernel.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-Microkernel.png" alt="Ambiguous-Microkernel" loading="lazy" width="663" height="607" style="width:80%"/>
</picture>
</a>
</figure>

*Microkernel* is another notable case\. The confusion over it goes all the way back to \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] which used [operating systems]({{< relref "../implementation-metapatterns/microkernel.md#operating-system" >}}) as examples of [*Plugin Architecture*]({{< relref "../implementation-metapatterns/plugins.md" >}})\. I believe that it was a mismatch:

- An operating system is mainly about sharing the resources of producers among consumers, where both the producers and consumers may be written by external teams\. The *kernel* itself does not feature much logic – its role is to connect the other components together\.
- *Plugins*, on the other hand, extend or modify the business logic of the *core* – which alone is the reason for the system to exist and is in no way “*micro\-*” as it got the bulk of the system’s code\. In many such systems *plugins* are utterly optional – which cannot be said of *OS drivers*\.


Thus, here we have two architectural patterns of arguably similar structure \([*Microkernel/Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) of \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] omit 3 of 5 components of the original [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) of \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\]\) but very different intent and action known under the same name\.

### Domain Services

<figure>
<a href="/diagrams/Conclusion/Ambiguous-DomainServices.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-DomainServices.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-DomainServices.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-DomainServices.png" alt="Ambiguous-DomainServices" loading="lazy" width="663" height="443" style="width:74%"/>
</picture>
</a>
</figure>

I was told that [*Domain Services*]({{< relref "../basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}}) of \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] are an incorrect term – because a *domain service* is always limited to the [*domain* layer]({{< relref "../basic-metapatterns/layers.md#domain-driven-design-ddd-layers" >}}) of \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] while those of \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] also cover the *application* and, maybe, *infrastructure*\.

I believe that both definitions are technically correct, if the difference in the meaning of *domain* is accounted for\. In \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] *domain* is almost synonymous with a *bounded context* of \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\], while \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] more often uses that word for the name of its middle layer which contains *business rules*\.

### Service\-Based Architecture

<figure>
<a href="/diagrams/Conclusion/Ambiguous-ServiceBasedArchitecture.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-ServiceBasedArchitecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-ServiceBasedArchitecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-ServiceBasedArchitecture.png" alt="Ambiguous-ServiceBasedArchitecture" loading="lazy" width="1401" height="442" style="width:100%"/>
</picture>
</a>
</figure>

\[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\] calls anything service\-based a *service\-based architecture*\.

\[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] differentiates [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) and [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), leaving whatever remains \(large [subdomain\-scale services]({{< relref "../basic-metapatterns/services.md#whole-subdomain-sub-domain-services" >}})\) under the name of [*Service\-Based Architecture*]({{< relref "../basic-metapatterns/services.md#service-based-architecture-sba" >}})\.

Both definitions are technically correct\. One is wider than the other\.

### Front Controller

<figure>
<a href="/diagrams/Conclusion/Ambiguous-FrontController.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-FrontController.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-FrontController.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-FrontController.png" alt="Ambiguous-FrontController" loading="lazy" width="668" height="383" style="width:70%"/>
</picture>
</a>
</figure>

\[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}}) and [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] define [*Front Controller*](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/ff648617(v=pandp.10)?redirectedfrom=MSDN) as an [*MVC*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) derivative for backend programming\. In this pattern multiple web pages share a request processing component which turns the incoming requests into commands and forwards them to appropriate page classes\.

The definition from \[[SAHP]({{< relref "../appendices/books-referenced.md#sahp" >}})\] is much more interesting – it describes an [*Choreographed Event\-Driven Architecture*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) with a [*Query Service*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) embedded in the first \(client\-facing\) service\. The [*Front Controller*]({{< relref "../extension-metapatterns/combined-component.md#front-controller" >}}) subscribes to notifications from downstream services to know the status of every request it has passed to the [*pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.

### Cells

<figure>
<a href="/diagrams/Conclusion/Ambiguous-Cells.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-Cells.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-Cells.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-Cells.png" alt="Ambiguous-Cells" loading="lazy" width="1203" height="463" style="width:100%"/>
</picture>
</a>
</figure>

The fresh *Cell\-Based Architecture* also has multiple definitions\. 

- WSO2 [wrote](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) about a [*Cell*]({{< relref "../basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) as a group of services which is encapsulated from the remaining system by a [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(for incoming traffic\) and sometimes *Adapters* \(for outgoing traffic\) and often uses a dedicated [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) – causing each *Cell*, though internally distributed, to be treated by other components as a single service\. This makes designing and managing a large system a bit simpler by introducing a [*hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\.
- Amazon [promotes](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/what-is-a-cell-based-architecture.html) its [*Cells*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) as [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}) of the whole system which run in multiple geographic regions\. That grants fault tolerance and improves performance as each client has an instance of the system deployed to a nearby datacenter, but it does not have much impact on organization and complexity of the code\.


This case looks like Amazon’s hijacking and redefining a popular emerging technology, though I may be wrong about that as I did not investigate the history of the term\. 

### Nanoservices

<figure>
<a href="/diagrams/Conclusion/Ambiguous-Nanoservices.png">
<picture>
<source srcset="/diagrams/Conclusion/Ambiguous-Nanoservices.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Conclusion/Ambiguous-Nanoservices.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Conclusion/Ambiguous-Nanoservices.png" alt="Ambiguous-Nanoservices" loading="lazy" width="1263" height="883" style="width:100%"/>
</picture>
</a>
</figure>

The *Nanoservices* pattern is another emerging technology and it seems to have never been strictly defined\. Most sources agree that a [*nanoservice*]({{< relref "../basic-metapatterns/services.md#single-function-faas-nanoservices" >}}) is a cloud\-based function \([*FaaS*](https://en.wikipedia.org/wiki/Function_as_a_service)\), similar to a *service* with a single API method but, just as with the old good [*services*]({{< relref "../basic-metapatterns/services.md" >}}), they differ in the ways they use the novel technology:

- Diego Zanon in *Building Serverless Web Applications* proposes a [single layer of nanoservices]({{< relref "../basic-metapatterns/services.md#inexact-nanoservices-api-layer" >}}), each implementing a method of the system’s public API, to be used as a thin backend\.
- [Here](https://increment.com/software-architecture/the-rise-of-nanoservices/) we have nanoservices [built into]({{< relref "../basic-metapatterns/pipeline.md#function-as-a-service-faas-nanoservices-pipelined" >}}) a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}), similar to [*Choreographed Event\-Driven Architecture*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\. 
- [Another article](https://medium.com/@ido.vapner/unlocking-the-power-of-nano-services-a-new-era-in-microservices-architecture-22647ea36f22) proposes to [\(re\)use them]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#nanoservices" >}}) in [*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) style\.


Moreover, there are a couple of sources that call a nanoservice something totally different:

- [There is a concept](https://nanoservices.io/docs/docs/building/introduction/) of nanoservice as a module that can run both as a separate service and as a part of a binary – allowing for a team to choose if they want their system to run as a single process or become distributed\. *Nano\-* is because an in\-process module is more lightweight than a [*microservice*]({{< relref "../basic-metapatterns/services.md#microservices" >}})\. This idea resembles [*Modular Monolith*]({{< relref "../basic-metapatterns/services.md#asynchronous-modules-modular-monolith-modulith-embedded-actors" >}}) \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] and [*Actor Frameworks*]({{< relref "../basic-metapatterns/services.md#actors" >}})\.
- And [here](https://dev.to/siy/nanoservices-or-alternative-to-monoliths-and-microservices-12bb) we have something akin to [*Space\-Based Architecture*]({{< relref "../implementation-metapatterns/mesh.md#space-based-architecture" >}}) but it is also called *Nanoservices* – as the proposed framework makes new components so easy to create that programmers tend to write many smaller *nanoservices* instead of a single [*microservice*]({{< relref "../basic-metapatterns/services.md#part-of-a-subdomain-microservices" >}})\.


In my opinion, the disarray happened because the notion of *making smaller microservices* got hyped but was never adopted widely enough to become an industry standard, therefore everybody follows their own vision about what *smaller* means\.

### Summary

A few names of architectural patterns cause confusion as the meaning of each of them changes from source to source\. This book aims at identifying such issues and building a cohesive understanding of software and system architecture, similar to the *ubiquitous language* of \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\.