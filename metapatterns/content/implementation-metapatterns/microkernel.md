+++
weight = 7
title = "Microkernel"
description = "A microkernel mediates between resource providers and resource consumers. It both makes the providers expendable and sandboxes the consumers."
images = ["/diagrams/Main/Microkernel.svg"]
[sitemap]
  priority = 0.8
+++

# Microkernel

<figure>
<a href="/diagrams/Main/Microkernel.png">
<picture>
<source srcset="/diagrams/Main/Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Microkernel.png" alt="Microkernel" loading="lazy" width="844" height="534" style="width:100%"/>
</picture>
</a>
</figure>

*Communism\.* Share resources among consumers\.

<ins>Known as:</ins> Microkernel \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}) but [not]({{< relref "../analytics/ambiguous-patterns.md#microkernel" >}}) [SAP]({{< relref "../appendices/books-referenced.md#sap" >}}) and [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\.

<ins>Aspects:</ins>

- [Middleware]({{< relref "../extension-metapatterns/middleware.md" >}}),
- [Orchestrator]({{< relref "../extension-metapatterns/orchestrator.md" >}})\.


<ins>Variants:</ins>

- Operating System,
- Software Framework,
- Virtualizer / Hypervisor / Container Orchestrator \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / Distributed Runtime,
- Interpreter \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] / Script / Domain\-Specific Language \(DSL\),
- Configurator / Configuration File,
- Saga Engine,
- [AUTOSAR Classic Platform](https://www.autosar.org/fileadmin/standards/R20-11/CP/AUTOSAR_EXP_VFB.pdf)\.


<ins>Structure:</ins> A layer of [*Orchestrators*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) over a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) over a layer of [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.

<ins>Type:</ins> Implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| The system’s complexity is evenly distributed among the components | The API and SPIs are very hard to change |
| Polymorphism of the resource providers | Performance is suboptimal |
| The components can have independent qualities | Latency is often unpredictable |
| A resource provider can be implemented and tested in isolation |  |
| Each application is sandboxed by the microkernel |  |
| The system is platform\-independent |  |

<ins>References:</ins> Microkernel pattern in \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\]\.

While vanilla [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) and [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) keep the business logic in the monolithic *core* component, *Microkernel* treats the core as a thin [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) \(called *microkernel*\) that connects user\-facing applications \(*external services*\) to resource providers \(*internal services*\)\. The *resource* in question can be anything ranging from CPU time or RAM to business functions\. The external services communicate with the microkernel through its *API* while the internal services implement the microkernel's *service provider interfaces* \(*SPIs*\) \(usually there is a kind of internal service and an SPI per resource type\)\.

On one hand, the pattern is very specific and feels esoteric\. On the other – it is indispensable in many domains, with many more real\-life occurrences than would be expected\. *Microkernel* finds its place where there are a variety of applications that need to use multiple shared resources, with each resource being independent of others and requiring complex management\.

### Performance

The *microkernel*, being an extra layer of indirection, degrades performance\. The actual extent varies from a few percent for *OSes* and *virtualizers* to an order of magnitude for *scripts*\. A more grievous aspect of performance is that latency becomes unpredictable as soon as the system runs short of one of the shared resources: memory, disk space, CPU time, or even storage for deleted objects\. That is why [real\-time systems]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) rely on minimalistic [real\-time OS](https://en.wikipedia.org/wiki/Real-time_operating_system)es or even run on bare metal\.

It is common to see system components communicate directly via shared memory or sockets bypassing the *microkernel* to alleviate the performance penalty it introduces\.

### Dependencies

The *applications* depend on the *API* of the *microkernel* while the *providers* depend on its *SPIs*\. On one hand, that isolates the applications and providers from each other, letting them develop independently\. On the other hand, the microkernel’s API and SPIs should be very stable to support older versions of the components which the microkernel integrates\.

<figure>
<a href="/diagrams/Dependencies/Microkernel.png">
<picture>
<source srcset="/diagrams/Dependencies/Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Microkernel.png" alt="Microkernel" loading="lazy" width="843" height="283" style="width:100%"/>
</picture>
</a>
</figure>

### Applicability

*Microkernel* is <ins>applicable</ins> in:

- *System programming\.* You manage system resources and services which will be used by untrusted client applications\. Hide the real resources behind a trusted proxy layer\. Be ready to change the hardware platform without affecting existing client code\.
- *Frameworks that integrate several subdomains\.* The microkernel component coordinates multiple specialized libraries\. Its API is a *Facade* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] for the managed functionality\.
- *Scripting or* [*DSL*](https://en.wikipedia.org/wiki/Domain-specific_language)*s\.* The microkernel is an *Interpreter* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] which lets your clients’ code manage the underlying system\.


*Microkernel* <ins>does not fit</ins>:

- *Coupled domains\.* Any degree of coupling between the resource providers complicates the microkernel and its SPIs and is likely to degrade performance which, however, may be salvaged by introducing direct communication channels between the providers\.


### Relations

<figure>
<a href="/diagrams/Relations/Microkernel.png">
<picture>
<source srcset="/diagrams/Relations/Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Microkernel.png" alt="Microkernel" loading="lazy" width="1143" height="523" style="width:100%"/>
</picture>
</a>
</figure>

*Microkernel*:

- Is a specialization of [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}})\.
- Is related to [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), which is a layer of [*Orchestrators*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) over a layer of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}); *Microkernel* adds a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) in between\.
- Is a kind of 2\-layered [*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) with an [*ESB*]({{< relref "../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\.
- The *microkernel* layer serves as \(implements\) a [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) for the upper \(*external*\) [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) and often makes an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) for the lower \(*internal*\) *Services*\.
- May be implemented by [*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}})\.


## Variants

*Microkernel* can appear in many forms:

### Operating System

<figure>
<a href="/diagrams/Variants/4/OS.png">
<picture>
<source srcset="/diagrams/Variants/4/OS.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/OS.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/OS.png" alt="OS" loading="lazy" width="903" height="373" style="width:100%"/>
</picture>
</a>
</figure>

The original inspiration for *Microkernel*, namely *operating systems*, provides an almost perfect example of the pattern, even though their kernels are not that “micro\-” \(unless you are running [MINIX](https://en.wikipedia.org/wiki/Minix_3#Architecture) or [QNX](https://en.wikipedia.org/wiki/QNX#Technology)\)\. [Device *drivers*]({{< relref "../basic-metapatterns/services.md#inexact-device-drivers" >}}) \(*internal services*\) encapsulate available hardware resources and make them accessible to user\-space *applications* \(*external services*\) via an OS *kernel*\. *Drivers* for a given kind of subsystem \(e\.g\. network adapter or disk drive\) are polymorphic towards the kernel and match the hardware installed\. 

### Software Framework

<figure>
<a href="/diagrams/Variants/4/Framework.png">
<picture>
<source srcset="/diagrams/Variants/4/Framework.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Framework.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Framework.png" alt="Framework" loading="lazy" width="903" height="303" style="width:100%"/>
</picture>
</a>
</figure>

The *microkernel* is a [*Facade*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] that integrates a set of libraries and exposes a user\-friendly high\-level interface\. [PAM](https://docs.oracle.com/cd/E23824_01/html/819-2145/pam-01.html) looks like a reasonably good example\.

### Virtualizer, Hypervisor, Container Orchestrator, Distributed Runtime

<figure>
<a href="/diagrams/Variants/4/Virtualizer.png">
<picture>
<source srcset="/diagrams/Variants/4/Virtualizer.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Virtualizer.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Virtualizer.png" alt="Virtualizer" loading="lazy" width="840" height="383" style="width:100%"/>
</picture>
</a>
</figure>

*Hypervisors* \(Xen\), PaaS and [FaaS](https://shivangsnewsletter.com/p/why-doesnt-cloudflare-use-containers), *container orchestrators* \(Kubernetes\) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\], and distributed *actor frameworks* \(Akka, Erlang/Elixir/OTP\) use resources of the underlying computer\(s\) to run guest applications\. A hypervisor virtualizes the resources of a single computer while a distributed runtime manages those of multiple servers – in the last case there are several instances of the same kind of an *internal server* which abstracts a host system\.

### Interpreter, Script, Domain\-Specific Language \(DSL\)

<figure>
<a href="/diagrams/Variants/4/Interpreter.png">
<picture>
<source srcset="/diagrams/Variants/4/Interpreter.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Interpreter.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Interpreter.png" alt="Interpreter" loading="lazy" width="902" height="303" style="width:100%"/>
</picture>
</a>
</figure>

User\-provided *scripts* are run by an *Interpreter* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] which also allows them to access a set of installed libraries\. The *Interpreter* is a microkernel, and the syntax of the script or [*DSL*](https://en.wikipedia.org/wiki/Domain-specific_language) it interprets is the microkernel’s API\.

### Configurator, Configuration File

<figure>
<a href="/diagrams/Variants/4/Config%20file.png">
<picture>
<source srcset="/diagrams/Variants/4/Config%20file.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Config%20file.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Config%20file.png" alt="Config file" loading="lazy" width="883" height="383" style="width:100%"/>
</picture>
</a>
</figure>

*Configuration files* may be regarded as short\-lived *scripts* that configure the underlying modules at the start of the system\. The parser of the configuration file is a transient *microkernel*\.

### Saga Engine

<figure>
<a href="/diagrams/Variants/4/Saga%20engine.png">
<picture>
<source srcset="/diagrams/Variants/4/Saga%20engine.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Saga%20engine.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Saga%20engine.png" alt="Saga engine" loading="lazy" width="943" height="384" style="width:100%"/>
</picture>
</a>
</figure>

A [*Saga*]({{< relref "../extension-metapatterns/orchestrator.md#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}) \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] orchestrates distributed transactions\. It may be written in a *DSL* which requires a compiler or interpreter, which is a *microkernel*, to execute\.

### AUTOSAR Classic Platform

<figure>
<a href="/diagrams/Variants/4/AUTOSAR%20classic.png">
<picture>
<source srcset="/diagrams/Variants/4/AUTOSAR%20classic.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/AUTOSAR%20classic.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/AUTOSAR%20classic.png" alt="AUTOSAR classic" loading="lazy" width="982" height="623" style="width:100%"/>
</picture>
</a>
</figure>

The [notorious](https://www.reddit.com/r/embedded/comments/leq366/comment/gmiq6d0/) [automotive standard](https://www.autosar.org/fileadmin/standards/R20-11/CP/AUTOSAR_EXP_VFB.pdf), though promoted as [*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#misapplied-automotive-soa" >}}), is structured as a distributed / virtualized *Microkernel*\. The application layer comprises a network of *software components* spread out over hundreds of chips which are, for some secret reason, called *electronic control units* \(*ECU*s\)\. The communication paths between the software components and much of the code are static \(auto\-generated at compilation time\)\. A software component may access hardware of its ECU via standard interfaces\.

The *microkernel* shows up as *Virtual Functional Bus* \(*VFB*\) which, as a *distributed* [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}), provides communication between the *applications* by virtualizing multiple *Runtime Environments* \(*RTEs*\) – the local [system interfaces](https://www.autosar.org/fileadmin/standards/R22-11/CP/AUTOSAR_EXP_LayeredSoftwareArchitecture.pdf)\.

## Summary

*Microkernel* is a ubiquitous approach to sharing resources among consumers, where both resource providers and consumers may be written by external companies\.