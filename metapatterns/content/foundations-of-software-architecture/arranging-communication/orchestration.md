+++
weight = 2
title = "Orchestration"
description = "A single component, called Orchestrator, coordinates other system components."
images = ["/diagrams/Web/og/Orchestration.png"]
[sitemap]
  priority = 0.5
+++

# Orchestration {anchor=false}

The most straightforward way to integrate services is to add a coordinating layer \([*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\) on top of them:

<figure>
<a href="/diagrams/Communication/Services%20to%20Orchestrator.png">
<picture>
<source srcset="/diagrams/Communication/Services%20to%20Orchestrator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Services%20to%20Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Services%20to%20Orchestrator.png" alt="Services to Orchestrator" loading="lazy" width="1063" height="293" style="width:100%"/>
</picture>
</a>
</figure>

The good thing is that your *Orchestrator* has explicit code for every use case it covers and every running scenario gets an associated thread, coroutine, or object so that you are able to attach to the *Orchestrator* and debug any use case step by step\. Nor do you have to worry about keeping the state of the services consistent as they are passive with all the changes in the system being driven by the *Orchestrator*\. 

Orchestration is the default approach for single\-process \(desktop\) applications where it is faster to call into an orchestrated module and return than to send it a message\. However, in distributed systems orchestration doubles the communication overhead \(when compared to [choreography]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) or [shared data]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\) as every method call into an orchestrated service uses two messages: request and confirmation\.

## Roles

In a backend which serves client requests an *Orchestrator* takes the role of [*Facade*](https://refactoring.guru/design-patterns/facade) \[[GoF]({{< relref "../../appendices/books-referenced.md#gof" >}})\] – a module that provides and implements a high\-level interface for a multicomponent system\. It sends requests to the underlying services and waits for their confirmations – the mode of action that can be wrapped in an [*RPC*](https://en.wikipedia.org/wiki/Remote_procedure_call) \(*remote procedure call*\)\. The state of each scenario that the facade runs resides in the associated thread’s or coroutine’s call stack \(for [*Reactor*]({{< relref "../../basic-metapatterns/monolith.md#single-threaded-reactor-one-thread-one-task" >}}) \[[POSA2]({{< relref "../../appendices/books-referenced.md#posa2" >}})\] or [*Half\-Sync/Half\-Async*]({{< relref "../../basic-metapatterns/monolith.md#inexact-half-synchalf-async-coroutines-or-fibers" >}}) \[[POSA2]({{< relref "../../appendices/books-referenced.md#posa2" >}})\] implementations, respectively\) or in a dedicated object \(for [*Proactor*]({{< relref "../../basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) \[[POSA2]({{< relref "../../appendices/books-referenced.md#posa2" >}})\]\)\.

<figure>
<a href="/diagrams/Communication/Facade.png">
<picture>
<source srcset="/diagrams/Communication/Facade.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Facade.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Facade.png" alt="Facade" loading="lazy" width="983" height="303" style="width:100%"/>
</picture>
</a>
</figure>

A *Facade* also supports querying the services in parallel and collecting the data returned into a single message through the *Splitter* and *Aggregator* patterns of \[[EIP]({{< relref "../../appendices/books-referenced.md#eip" >}})\]\. That reduces latency \(and resource consumption as the whole task is completed faster\) for [scatter or gather](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) requests when compared to sequential execution\.

<figure>
<a href="/diagrams/Communication/Facade%20-%20Parallel.png">
<picture>
<source srcset="/diagrams/Communication/Facade%20-%20Parallel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Facade%20-%20Parallel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Facade%20-%20Parallel.png" alt="Facade - Parallel" loading="lazy" width="783" height="303" style="width:90%"/>
</picture>
</a>
</figure>

Embedded and system programming – the areas that deal with automating [*control*]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) of hardware or distributed software – employ *Orchestrators* as [*Mediators*](https://refactoring.guru/design-patterns/mediator) \[[GoF]({{< relref "../../appendices/books-referenced.md#gof" >}})\]  – components that keep the state of the whole system \(and, by implication, any hardware it may manage\) consistent by enacting a system\-wide reaction to any observable change in any of the system’s constituents\. A mediator operates in non\-blocking, fire\-and\-forget mode which is more characteristic of choreography, to be discussed [below]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}})\. This also means that you will not be able to debug a use case as a thread – because [there are no predefined scenarios in control software](https://medium.com/itnext/control-and-processing-software-9011fee8bc66)\!

<figure>
<a href="/diagrams/Communication/Mediator.png">
<picture>
<source srcset="/diagrams/Communication/Mediator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mediator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mediator.png" alt="Mediator" loading="lazy" width="903" height="303" style="width:100%"/>
</picture>
</a>
</figure>

Such a difference may be rooted in the direction of the control and information flow: in a backend it comes as a high\-level command while control systems react to low\-level events\.

## Dependencies

By default an *Orchestrator* depends on each service which it manages – that means that a change in a service’s interface or contract – caused by fixing a bug, adding a feature, or optimizing performance – requires corresponding changes in the *Orchestrator*\. That is acceptable as the *Orchestrator*’s client\-facing, high\-level logic tends to evolve much faster than the business rules of the lower layer of services, therefore the team behind the orchestrator, unrestricted by other components depending on it, will likely release way more often than any other team\. However, as the number of the managed services and the lengths of their APIs increase, so does the amount of information that the *Orchestrator*’s team must remember and the influx of changes they must integrate in their code\. For a large project the workload of supporting the *orchestration layer* may paralyze its development – that was a major reason behind the decline of [*Enterprise SOA*]({{< relref "../../fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) \[[FSA]({{< relref "../../appendices/books-referenced.md#fsa" >}})\] where [*ESB*]({{< relref "../../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) used to orchestrate all the interactions in the system, including those between domain\-level services and components of the utility layer\.

<figure>
<a href="/diagrams/Communication/Orchestrator%20-%20Dependencies.png">
<picture>
<source srcset="/diagrams/Communication/Orchestrator%20-%20Dependencies.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Orchestrator%20-%20Dependencies.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Orchestrator%20-%20Dependencies.png" alt="Orchestrator - Dependencies" loading="lazy" width="943" height="205" style="width:100%"/>
</picture>
</a>
</figure>

Another option, which appears in [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) and develops in [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) and [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) stems from [*dependency inversion*](https://en.wikipedia.org/wiki/Dependency_inversion_principle): the *Orchestrator* defines an [*SPI*](https://en.wikipedia.org/wiki/Service_provider_interface) \(*service provider interface*\) for every service\. That makes each service depend on the *Orchestrator* so that a single *Orchestrator*’s team does not need to follow updates of the multiple services’ APIs – instead it initiates the changes at its own pace\. However, with that approach the design of an SPI requires coordination from the teams on both sides of it and the once settled interface becomes hard to change\. The most famous example of modules that implement SPIs are OS drivers\.

<figure>
<a href="/diagrams/Communication/Microkernel%20-%20Dependencies.png">
<picture>
<source srcset="/diagrams/Communication/Microkernel%20-%20Dependencies.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Microkernel%20-%20Dependencies.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Microkernel%20-%20Dependencies.png" alt="Microkernel - Dependencies" loading="lazy" width="963" height="205" style="width:100%"/>
</picture>
</a>
</figure>

Furthermore, some domains develop that idea into a [*Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}): when services implement related concepts, they may match a single SPI, making the *Orchestrator* simpler \(as there is no more need to remember multiple interfaces\)\. That is the case with telecom or payment gateways and it may also be found with trees of product categories in online marketplaces\.

<figure>
<a href="/diagrams/Communication/Hierarchy%20-%20Dependencies.png">
<picture>
<source srcset="/diagrams/Communication/Hierarchy%20-%20Dependencies.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Hierarchy%20-%20Dependencies.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Hierarchy%20-%20Dependencies.png" alt="Hierarchy - Dependencies" loading="lazy" width="903" height="243" style="width:100%"/>
</picture>
</a>
</figure>

All kinds of orchestration allow for an easy addition of new use cases which may even involve new services as that changes nothing in the existing code\. However, removing or restructuring \(splitting or merging\) previously integrated services requires much work within the orchestrator, except for in a *Hierarchy* where all the services implement the same interface which means that the code in the *Orchestrator* does not depend \(much\) on any specific child\.

<figure>
<a href="/diagrams/Communication/Orchestrator%20add%20a%20Use%20Case.png">
<picture>
<source srcset="/diagrams/Communication/Orchestrator%20add%20a%20Use%20Case.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Orchestrator%20add%20a%20Use%20Case.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Orchestrator%20add%20a%20Use%20Case.png" alt="Orchestrator add a Use Case" loading="lazy" width="1123" height="303" style="width:100%"/>
</picture>
</a>
</figure>

## Mutual orchestration

In some systems there are several services that have their own kinds of clients \(for example, employees of different departments\)\. Each of the services tries hard to process its clients’ requests on its own but occasionally still needs help from other parts of the system\. This creates a paradoxical case where several services orchestrate each other:

<figure>
<a href="/diagrams/Communication/Mutual%20Orchestration%20-%201.png">
<picture>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%201.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%201.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mutual%20Orchestration%20-%201.png" alt="Mutual Orchestration - 1" loading="lazy" width="883" height="263" style="width:100%"/>
</picture>
</a>
</figure>

As each of the services depends on the APIs of the others, any change to any interface or composition of such a system requires consent and collaboration from every team as it impacts the code of all the services\.

<figure>
<a href="/diagrams/Communication/Mutual%20Orchestration%20-%202.png">
<picture>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%202.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mutual%20Orchestration%20-%202.png" alt="Mutual Orchestration - 2" loading="lazy" width="883" height="185" style="width:100%"/>
</picture>
</a>
</figure>

In real life services are likely to be layered, with their upper layers acting as both internal and external *Orchestrators*\. Layering isolates interdependencies to the relatively small application\-level components and resolves, to an extent, the seemingly counterintuitive case of mutual orchestration as now there is an explicit, though fragmented, system\-wide orchestration layer\.

<figure>
<a href="/diagrams/Communication/Mutual%20Orchestration%20-%203.png">
<picture>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%203.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%203.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mutual%20Orchestration%20-%203.png" alt="Mutual Orchestration - 3" loading="lazy" width="1023" height="363" style="width:100%"/>
</picture>
</a>
</figure>

<figure>
<a href="/diagrams/Communication/Mutual%20Orchestration%20-%204.png">
<picture>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%204.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Mutual%20Orchestration%20-%204.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Mutual%20Orchestration%20-%204.png" alt="Mutual Orchestration - 4" loading="lazy" width="1023" height="294" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

Orchestration represents use cases as a code, allowing for an orchestrated system to support many complex scenarios\. Dealing with errors is as trivial as properly handling exceptions\. This approach trades performance for clarity\.