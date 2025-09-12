+++
weight = 7
title = "Microkernel"
+++

# Microkernel

<figure style="text-align:center">
<a href="/Main/Microkernel.png" style="outline:none">
<img src="/Main/Microkernel.png" alt="Microkernel" style="width:100%"/>
</a>
</figure>

*Communism\.* Share resources among consumers\.

<ins>Known as:</ins> Microkernel \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}}), [POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa4" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#microkernel" >}}) [SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sap" >}}) and [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]\.

<ins>Aspects:</ins>

- [Middleware]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}),
- [Orchestrator]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.


<ins>Variants:</ins>

- Operating System,
- Software Framework,
- Virtualizer / Hypervisor / Container Orchestrator \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] / Distributed Runtime,
- Interpreter \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] / Script / Domain\-Specific Language \(DSL\),
- Configurator / Configuration File,
- Saga Engine,
- [AUTOSAR Classic Platform](https://www.autosar.org/fileadmin/standards/R20-11/CP/AUTOSAR_EXP_VFB.pdf)\.


<ins>Structure:</ins> A layer of [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) over a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) over a layer of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}})\.

<ins>Type:</ins> Implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| The system’s complexity is evenly distributed among the components | The API and SPIs are very hard to change |
| Polymorphism of the resource providers | Performance is suboptimal |
| The components can have independent qualities | Latency is often unpredictable |
| A resource provider can be implemented and tested in isolation |  |
| Each application is sandboxed by the microkernel |  |
| The system is platform\-independent |  |


<ins>References:</ins> Microkernel pattern in \[[POSA1]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}})\]\.

While vanilla [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) and [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) keep the business logic in the monolithic *core* component, *Microkernel* treats the core as a thin [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) \(called *microkernel*\) that connects user\-facing applications \(*external services*\) to resource providers \(*internal services*\)\. The *resource* in question can be anything ranging from CPU time or RAM to business functions\. The external services communicate with the microkernel through its *API* while the internal services implement the microkernel's *service provider interfaces* \(*SPIs*\) \(usually there is a kind of internal service and an SPI per resource type\)\.

On one hand, the pattern is very specific and feels esoteric\. On the other – it is indispensable in many domains, with many more real\-life occurrences than would be expected\. *Microkernel* finds its place where there are a variety of applications that need to use multiple shared resources, with each resource being independent of others and requiring complex management\.

### Performance

The *microkernel*, being an extra layer of indirection, degrades performance\. The actual extent varies from a few percent for *OSes* and *virtualizers* to an order of magnitude for *scripts*\. A more grievous aspect of performance is that latency becomes unpredictable as soon as the system runs short of one of the shared resources: memory, disk space, CPU time, or even storage for deleted objects\. That is why [real\-time systems]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) rely on minimalistic [real\-time OS](https://en.wikipedia.org/wiki/Real-time_operating_system)es or even run on bare metal\.

It is common to see system components communicate directly via shared memory or sockets bypassing the *microkernel* to alleviate the performance penalty it introduces\.

### Dependencies

The *applications* depend on the *API* of the *microkernel* while the *providers* depend on its *SPIs*\. On one hand, that isolates the applications and providers from each other, letting them develop independently\. On the other hand, the microkernel’s API and SPIs should be very stable to support older versions of the components which the microkernel integrates\.

<figure style="text-align:center">
<a href="/Dependencies/Microkernel.png" style="outline:none">
<img src="/Dependencies/Microkernel.png" alt="Microkernel" style="width:100%"/>
</a>
</figure>

### Applicability

*Microkernel* is <ins>applicable</ins> in:

- *System programming\.* You manage system resources and services which will be used by untrusted client applications\. Hide the real resources behind a trusted proxy layer\. Be ready to change the hardware platform without affecting existing client code\.
- *Frameworks that integrate several subdomains\.* The microkernel component coordinates multiple specialized libraries\. Its API is a *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] for the managed functionality\.
- *Scripting or* [*DSL*](https://en.wikipedia.org/wiki/Domain-specific_language)*s\.* The microkernel is an *Interpreter* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] which lets your clients’ code manage the underlying system\.


*Microkernel* <ins>does not fit</ins>:

- *Coupled domains\.* Any degree of coupling between the resource providers complicates the microkernel and its SPIs and is likely to degrade performance which, however, may be salvaged by introducing direct communication channels between the providers\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Microkernel.png" style="outline:none">
<img src="/Relations/Microkernel.png" alt="Microkernel" style="width:100%"/>
</a>
</figure>

*Microkernel*:

- Is a specialization of [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\.
- Is related to [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), which is a layer of [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) over a layer of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}); *Microkernel* adds a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) in between\.
- Is a kind of 2\-layered [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) with an [*ESB*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\.
- The *microkernel* layer serves as \(implements\) a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) for the upper \(*external*\) [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) and often makes an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) for the lower \(*internal*\) *Services*\.
- May be implemented by [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\.


## Variants

*Microkernel* can appear in many forms:

### Operating System

<figure style="text-align:center">
<a href="/Variants/4/OS.png" style="outline:none">
<img src="/Variants/4/OS.png" alt="OS" style="width:100%"/>
</a>
</figure>

The original inspiration for *Microkernel*, namely *operating systems*, provides an almost perfect example of the pattern, even though their kernels are not that “micro\-” \(unless you are running [MINIX](https://en.wikipedia.org/wiki/Minix_3#Architecture) or [QNX](https://en.wikipedia.org/wiki/QNX#Technology)\)\. [Device *drivers*]({{< relref "../part-2--basic-metapatterns/services.md#inexact-device-drivers" >}}) \(*internal services*\) encapsulate available hardware resources and make them accessible to user\-space *applications* \(*external services*\) via an OS *kernel*\. *Drivers* for a given kind of subsystem \(e\.g\. network adapter or disk drive\) are polymorphic towards the kernel and match the hardware installed\. 

### Software Framework

<figure style="text-align:center">
<a href="/Variants/4/Framework.png" style="outline:none">
<img src="/Variants/4/Framework.png" alt="Framework" style="width:100%"/>
</a>
</figure>

The *microkernel* is a [*Facade*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] that integrates a set of libraries and exposes a user\-friendly high\-level interface\. [PAM](https://docs.oracle.com/cd/E23824_01/html/819-2145/pam-01.html) looks like a reasonably good example\.

### Virtualizer, Hypervisor, Container Orchestrator, Distributed Runtime

<figure style="text-align:center">
<a href="/Variants/4/Virtualizer.png" style="outline:none">
<img src="/Variants/4/Virtualizer.png" alt="Virtualizer" style="width:100%"/>
</a>
</figure>

*Hypervisors* \(Xen\), PaaS and [FaaS](https://shivangsnewsletter.com/p/why-doesnt-cloudflare-use-containers), *container orchestrators* \(Kubernetes\) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\], and distributed *actor frameworks* \(Akka, Erlang/Elixir/OTP\) use resources of the underlying computer\(s\) to run guest applications\. A hypervisor virtualizes the resources of a single computer while a distributed runtime manages those of multiple servers – in the last case there are several instances of the same kind of an *internal server* which abstracts a host system\.

### Interpreter, Script, Domain\-Specific Language \(DSL\)

<figure style="text-align:center">
<a href="/Variants/4/Interpreter.png" style="outline:none">
<img src="/Variants/4/Interpreter.png" alt="Interpreter" style="width:100%"/>
</a>
</figure>

User\-provided *scripts* are run by an *Interpreter* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] which also allows them to access a set of installed libraries\. The *Interpreter* is a microkernel, and the syntax of the script or [*DSL*](https://en.wikipedia.org/wiki/Domain-specific_language) it interprets is the microkernel’s API\.

### Configurator, Configuration File

<figure style="text-align:center">
<a href="/Variants/4/Config%20file.png" style="outline:none">
<img src="/Variants/4/Config%20file.png" alt="Config file" style="width:100%"/>
</a>
</figure>

*Configuration files* may be regarded as short\-lived *scripts* that configure the underlying modules at the start of the system\. The parser of the configuration file is a transient *microkernel*\.

### Saga Engine

<figure style="text-align:center">
<a href="/Variants/4/Saga%20engine.png" style="outline:none">
<img src="/Variants/4/Saga%20engine.png" alt="Saga engine" style="width:100%"/>
</a>
</figure>

A [*Saga*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] orchestrates distributed transactions\. It may be written in a *DSL* which requires a compiler or interpreter, which is a *microkernel*, to execute\.

### AUTOSAR Classic Platform

<figure style="text-align:center">
<a href="/Variants/4/AUTOSAR%20classic.png" style="outline:none">
<img src="/Variants/4/AUTOSAR%20classic.png" alt="AUTOSAR classic" style="width:100%"/>
</a>
</figure>

The [notorious](https://www.reddit.com/r/embedded/comments/leq366/comment/gmiq6d0/) [automotive standard](https://www.autosar.org/fileadmin/standards/R20-11/CP/AUTOSAR_EXP_VFB.pdf), though promoted as [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#misapplied-automotive-soa" >}}), is structured as a distributed / virtualized *Microkernel*\. The application layer comprises a network of *software components* spread out over hundreds of chips which are, for some secret reason, called *electronic control units* \(*ECU*s\)\. The communication paths between the software components and much of the code are static \(auto\-generated at compilation time\)\. A software component may access hardware of its ECU via standard interfaces\.

The *microkernel* shows up as *Virtual Functional Bus* \(*VFB*\) which, as a *distributed* [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}), provides communication between the *applications* by virtualizing multiple *Runtime Environments* \(*RTEs*\) – the local [system interfaces](https://www.autosar.org/fileadmin/standards/R22-11/CP/AUTOSAR_EXP_LayeredSoftwareArchitecture.pdf)\.

## Summary

*Microkernel* is a ubiquitous approach to sharing resources among consumers, where both resource providers and consumers may be written by external companies\.

<nav>

| \<\< [Hexagonal Architecture]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) | ^ [Part 5\. Implementation Metapatterns]({{< relref "../part-5--implementation-metapatterns/_index.md" >}}) ^ | [Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) \>\> |
| --- | --- | --- |

</nav>



