+++
weight = 2
title = "Orchestration"
description = In orchestration there is a high-level component, called orchestrator, which uses (orchestrates) other components in order to implement application logic.
+++

# Orchestration

The most straightforward way to integrate services is to add a coordinating layer \([*Orchestrator*]({{< relref "../../part-3--extension-metapatterns/orchestrator.md" >}})\) on top of them:

<figure style="text-align:center">
<a href="/Communication/Services%20to%20Orchestrator.png" style="outline:none">
<img src="/Communication/Services%20to%20Orchestrator.png" alt="Services to Orchestrator" style="width:100%"/>
</a>
</figure>

The good thing is that your *Orchestrator* has explicit code for every use case it covers and every running scenario gets an associated thread, coroutine, or object so that you are able to attach to the *Orchestrator* and debug any use case step by step\. Nor do you have to worry about keeping the state of the services consistent as they are passive with all the changes in the system being driven by the *Orchestrator*\. 

Orchestration is the default approach for single\-process \(desktop\) applications where it is faster to call into an orchestrated module and return than to send it a message\. However, in distributed systems orchestration doubles the communication overhead \(when compared to [choreography]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) or [shared data]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\) as every method call into an orchestrated service uses two messages: request and confirmation\.

## Roles

In a backend which serves client requests an *Orchestrator* takes the role of *Facade* \[[GoF]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] – a module that provides and implements a high\-level interface for a multicomponent system\. It sends requests to the underlying services and waits for their confirmations – the mode of action that can be wrapped in an [*RPC*](https://en.wikipedia.org/wiki/Remote_procedure_call) \(*remote procedure call*\)\. The state of each scenario that the facade runs resides in the associated thread’s or coroutine’s call stack \(for [*Reactor*]({{< relref "../../part-2--basic-metapatterns/monolith.md#single-threaded-reactor-one-thread-one-task" >}}) \[[POSA2]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#posa2" >}})\] or [*Half\-Sync/Half\-Async*]({{< relref "../../part-2--basic-metapatterns/monolith.md#inexact-half-synchalf-async-coroutines-or-fibers" >}}) \[[POSA2]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#posa2" >}})\] implementations, correspondingly\) or in a dedicated object \(for [*Proactor*]({{< relref "../../part-2--basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) \[[POSA2]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#posa2" >}})\]\)\.

<figure style="text-align:center">
<a href="/Communication/Facade.png" style="outline:none">
<img src="/Communication/Facade.png" alt="Facade" style="width:100%"/>
</a>
</figure>

A *Facade* also supports querying the services in parallel and collecting the data returned into a single message through the *Splitter* and *Aggregator* patterns of \[[EIP]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\]\. That reduces latency \(and resource consumption as the whole task is completed faster\) for [scatter or gather](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) requests when compared to sequential execution\.

<figure style="text-align:center">
<a href="/Communication/Facade%20-%20Parallel.png" style="outline:none">
<img src="/Communication/Facade%20-%20Parallel.png" alt="Facade - Parallel" style="width:90%"/>
</a>
</figure>

Embedded and system programming – the areas that deal with automating [*control*]({{< relref "../../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) of hardware or distributed software – employ *Orchestrators* as *Mediators* \[[GoF]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\]  – components that keep the state of the whole system \(and, by implication, any hardware it may manage\) consistent by enacting a system\-wide reaction to any observable change in any of the system’s constituents\. A mediator operates in non\-blocking, fire\-and\-forget mode which is more characteristic of choreography, to be discussed [below]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}})\. This also means that you will not be able to debug a use case as a thread – because [there are no predefined scenarios in control software](https://medium.com/itnext/control-and-processing-software-9011fee8bc66)\!

<figure style="text-align:center">
<a href="/Communication/Mediator.png" style="outline:none">
<img src="/Communication/Mediator.png" alt="Mediator" style="width:100%"/>
</a>
</figure>

Such a difference may be rooted in the direction of the control and information flow: in a backend it comes as a high\-level command while control systems react to low\-level events\.

## Dependencies

By default an *Orchestrator* depends on each service which it manages – that means that a change in a service’s interface or contract – caused by fixing a bug, adding a feature, or optimizing performance – requires corresponding changes in the *Orchestrator*\. That is acceptable as the *Orchestrator*’s client\-facing, high\-level logic tends to evolve much faster than the business rules of the lower layer of services, therefore the team behind the orchestrator, unrestricted by other components depending on it, will likely release way more often than any other team\. However, as the number of the managed services and the lengths of their APIs increase, so does the amount of information that the *Orchestrator*’s team must remember and the influx of changes they must integrate in their code\. For a large project the workload of supporting the *orchestration layer* may paralyze its development – that was a major reason behind the decline of [*Enterprise SOA*]({{< relref "../../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) \[[FSA]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] where [*ESB*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) used to orchestrate all the interactions in the system, including those between domain\-level services and components of the utility layer\.

<figure style="text-align:center">
<a href="/Communication/Orchestrator%20-%20Dependencies.png" style="outline:none">
<img src="/Communication/Orchestrator%20-%20Dependencies.png" alt="Orchestrator - Dependencies" style="width:100%"/>
</a>
</figure>

Another option, which appears in [*Plugins*]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}}) and develops in [*Microkernel*]({{< relref "../../part-5--implementation-metapatterns/microkernel.md" >}}) and [*Hexagonal Architecture*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) stems from [*dependency inversion*](https://en.wikipedia.org/wiki/Dependency_inversion_principle): the *Orchestrator* defines an [*SPI*](https://en.wikipedia.org/wiki/Service_provider_interface) \(*service provider interface*\) for every service\. That makes each service depend on the *Orchestrator* so that a single *Orchestrator*’s team does not need to follow updates of the multiple services’ APIs – instead it initiates the changes at its own pace\. However, with that approach the design of an SPI requires coordination from the teams on both sides of it and the once settled interface becomes hard to change\. The most famous example of modules that implement SPIs are OS drivers\.

<figure style="text-align:center">
<a href="/Communication/Microkernel%20-%20Dependencies.png" style="outline:none">
<img src="/Communication/Microkernel%20-%20Dependencies.png" alt="Microkernel - Dependencies" style="width:100%"/>
</a>
</figure>

Furthermore, some domains develop that idea into a [*Hierarchy*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md" >}}): when services implement related concepts, they may match a single SPI, making the *Orchestrator* simpler \(as there is no more need to remember multiple interfaces\)\. That is the case with telecom or payment gateways and it may also be found with trees of product categories in online marketplaces\.

<figure style="text-align:center">
<a href="/Communication/Hierarchy%20-%20Dependencies.png" style="outline:none">
<img src="/Communication/Hierarchy%20-%20Dependencies.png" alt="Hierarchy - Dependencies" style="width:100%"/>
</a>
</figure>

All kinds of orchestration allow for an easy addition of new use cases which may even involve new services as that changes nothing in the existing code\. However, removing or restructuring \(splitting or merging\) previously integrated services requires much work within the orchestrator, except for in a *Hierarchy* where all the services implement the same interface which means that the code in the *Orchestrator* does not depend \(much\) on any specific child\.

<figure style="text-align:center">
<a href="/Communication/Orchestrator%20add%20a%20Use%20Case.png" style="outline:none">
<img src="/Communication/Orchestrator%20add%20a%20Use%20Case.png" alt="Orchestrator add a Use Case" style="width:100%"/>
</a>
</figure>

## Mutual orchestration

In some systems there are several services that have their own kinds of clients \(for example, employees of different departments\)\. Each of the services tries hard to process its clients’ requests on its own but occasionally still needs help from other parts of the system\. This creates a paradoxical case where several services orchestrate each other:

<figure style="text-align:center">
<a href="/Communication/Mutual%20Orchestration%20-%201.png" style="outline:none">
<img src="/Communication/Mutual%20Orchestration%20-%201.png" alt="Mutual Orchestration - 1" style="width:100%"/>
</a>
</figure>

As each of the services depends on the APIs of the others, any change to any interface or composition of such a system requires consent and collaboration from every team as it impacts the code of all the services\.

<figure style="text-align:center">
<a href="/Communication/Mutual%20Orchestration%20-%202.png" style="outline:none">
<img src="/Communication/Mutual%20Orchestration%20-%202.png" alt="Mutual Orchestration - 2" style="width:100%"/>
</a>
</figure>

In real life services are likely to be layered, with their upper layers acting as both internal and external *Orchestrators*\. Layering isolates interdependencies to the relatively small application\-level components and resolves, to an extent, the seemingly counterintuitive case of mutual orchestration as now there is an explicit, though fragmented, system\-wide orchestration layer\.

<figure style="text-align:center">
<a href="/Communication/Mutual%20Orchestration%20-%203.png" style="outline:none">
<img src="/Communication/Mutual%20Orchestration%20-%203.png" alt="Mutual Orchestration - 3" style="width:100%"/>
</a>
</figure>

<figure style="text-align:center">
<a href="/Communication/Mutual%20Orchestration%20-%204.png" style="outline:none">
<img src="/Communication/Mutual%20Orchestration%20-%204.png" alt="Mutual Orchestration - 4" style="width:100%"/>
</a>
</figure>

## Summary

Orchestration represents use cases as a code, allowing for an orchestrated system to support many complex scenarios\. Dealing with errors is as trivial as properly handling exceptions\. This approach trades performance for clarity\.

<nav>

| \<\< [Programming and architectural paradigms]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}}) | ^ [Arranging communication]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/_index.md" >}}) ^ | [Choreography]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) \>\> |
| --- | --- | --- |

</nav>



