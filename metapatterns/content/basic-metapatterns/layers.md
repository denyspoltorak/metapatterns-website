+++
weight = 8
title = "Layers"
description = "This chapter explores layered architectures (Layers and Tiers) and individual layers: interface, application, domain, utilities, middleware, and persistence."
images = ["/diagrams/Web/og/Layers.png"]
primary_image = "/diagrams/Main/Layers.png"
[sitemap]
  priority = 0.8
+++

# Layers {anchor=false}

<figure>
<a href="/diagrams/Main/Layers.png">
<picture>
<source srcset="/diagrams/Main/Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Layers.png" alt="A diagram for Layered Architecture, in abstractness-subdomain-sharding coordinates." loading="lazy" width="942" height="494" style="width:100%"/>
</picture>
</a>
</figure>

*Yet another layer of indirection\.* Don’t mix the business logic and implementation details\.

<ins>Known as:</ins> Layers \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\], Layered Architecture \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}}), [LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\], Multitier Architecture, and N\-tier Architecture \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\]\.

<ins>Structure:</ins> A component per level of abstractness\.

<ins>Type:</ins> System topology, implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Rapid start for development | Quickly deteriorates as the project grows |
| Easy debugging | Hard to develop with more than a few teams |
| Good performance | Does not solve force conflicts between subdomains |
| Development teams may specialize | Does not support aggressive optimizations |
| Business logic is encapsulated |  |
| Allows resolution of conflicting forces |  |
| Deployment to dedicated hardware |  |
| Layers with no business logic are reusable |  |

<ins>References:</ins> \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] and \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] discuss layered software in depth; \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] promotes the layered style; most of the architectures in Herberto Graça’s [Software Architecture Chronicles](https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/) are layered\. The Wiki has a reasonably [good article](https://en.wikipedia.org/wiki/Multitier_architecture)\.

*Layering* a system creates interfaces between its levels of abstractness \(high\-level [use cases]({{< relref "#application-use-cases-or-integration" >}}), lower\-level [domain logic]({{< relref "#domain-business-rules-or-model" >}}), infrastructure\) while also retaining monolithic cohesiveness within each of the levels\. That allows both for easy debugging inside each individual layer \(no need to jump into another programming language or re\-attach the debugger to a remote server\) and enough flexibility to have a dedicated development team, tools, deployment, and scaling policies for each\. Though layered code is slightly better than that of [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), thanks to the separation of concerns, one of the upper \(business logic\) layers may nonetheless grow too large for efficient development\.

Splitting a system into layers tends to resolve conflicts of forces between its abstract and optimized parts: the top\-level business logic changes rapidly and does not require much optimization \(as its methods are called infrequently\), thus it can be written in a high\-level programming language\. In contrast, infrastructure, which is called thousands of times per second, has stable workflows but must be thoroughly optimized and extremely well tested\.

Many patterns have one or more of their layers split by subdomain, resulting in a layer of *services*\. That causes no penalties as long as the services are completely independent \(the original layer had zero coupling between its subdomains\), which happens if each of them deals with a separate subset of requests \(as in [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\) or is choreographed by an upper layer \(as in [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}), [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) or [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\) which boils down to the same “separate subset of subrequests” under the hood\. However, if the services which form a layer need to intercommunicate, you immediately get a whole set of troubles with debugging, sharing data, and performance characteristic of the [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) architecture\.

<figure>
<a href="/diagrams/Misc/Layers%20of%20Services.png">
<picture>
<source srcset="/diagrams/Misc/Layers%20of%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Misc/Layers%20of%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Misc/Layers%20of%20Services.png" alt="Diagrams of Backends for Frontends and Services with Polyglot Persistence." loading="lazy" width="1643" height="477" style="width:100%"/>
</picture>
</a>
</figure>

<aside>

> Thanks to its substantial benefits and minor drawbacks, and the many evolutions it supports, *Layers* became the default architecture for starting new projects\. 

</aside>

### Performance

The performance of a layered system is shaped by two factors:

- Communication between layers is slower than within a layer\. Components of a layer may access each other’s data directly, while accessing another layer involves data transformation \(as interfaces tend to operate generic data structures\), serialization, and often IPC or networking\.
- The frequency and granularity of events or actions increases as we move from the upper more abstract layers to lower\-level components that interface an OS or hardware\.


<aside>

> An ideal component should be replaceable and reusable\. As soon as a component exposes details of its implementation, such as workflows or data types, in its interface, it becomes incompatible with other possible implementations, and its interface may even see major changes as the internals of the component evolve\. Therefore, well\-behaving components tend to have their interfaces written in most generic terms, which requires inputs to be transformed to their internal formats and thus penalizes performance\. 

</aside>

There is a number of optimizations to skip interlayer calls:

<figure>
<a href="/diagrams/Performance/Layers-caching.png">
<picture>
<source srcset="/diagrams/Performance/Layers-caching.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layers-caching.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layers-caching.png" alt="Caching the latest known state of the system in its highest layer." loading="lazy" width="1123" height="303" style="width:100%"/>
</picture>
</a>
</figure>

*Caching*: an upper layer tends to *model* \(cache last known state of\) the layers below it\. This way it can behave as if it knew the state of the whole system without querying the actual state from the hardware below all the layers\. Such an approach is universal for [*control software*]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\. For example, a network monitoring suite shows you the last known state of all the components it observes without actually querying them – it is subscribed to notifications and remembers what each device has previously reported\.

<figure>
<a href="/diagrams/Performance/Layers-aggregation.png">
<picture>
<source srcset="/diagrams/Performance/Layers-aggregation.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layers-aggregation.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layers-aggregation.png" alt="Aggregation of events from hardware by the lowest layer of a layered system." loading="lazy" width="1143" height="284" style="width:100%"/>
</picture>
</a>
</figure>

*Aggregation*: a lower layer collects multiple events before notifying the layer above it to avoid being overly chatty\. An example is an [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things) field gateway that collects data from all the sensors in the building and sends it in a single report to the server\. Or consider a data transfer over a network where a low\-level driver collects multiple data packets that come from the hardware and sends an acknowledgement for each of them while waiting for a datagram or file transfer to complete\. It notifies its client only once when all the data has been collected and its integrity confirmed\.

<figure>
<a href="/diagrams/Performance/Layers-batching.png">
<picture>
<source srcset="/diagrams/Performance/Layers-batching.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layers-batching.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layers-batching.png" alt="Sending a batch of commands all the way down to the lowest layer of a system." loading="lazy" width="1063" height="283" style="width:100%"/>
</picture>
</a>
</figure>

*Batching*: an upper layer forms a queue of commands and sends it as a single job to the layer below it\. This takes place in drivers for complex low\-level hardware, like printers, or in database access as *stored procedures*\. \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] describes the approach as *Combined Method*, *Enumeration Method* and *Batch Method* patterns\. Programming languages and frameworks may implement *foreach* and *map/reduce* which allow for a single command to operate on multiple pieces of data\.

<figure>
<a href="/diagrams/Performance/Layers-injection.png">
<picture>
<source srcset="/diagrams/Performance/Layers-injection.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Layers-injection.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Layers-injection.png" alt="Moving a part of the business logic from the highest layer to the lowest layer of the system." loading="lazy" width="1083" height="281" style="width:100%"/>
</picture>
</a>
</figure>

*Strategy injection*: an upper layer installs an event handler \(hook or [*Ambassador Plugin*]({{< relref "../implementation-metapatterns/plugins.md#ambassador-plugin-logic-extension" >}})\) into the lower layer\. The goal is for the hook to do basic pre\-processing, filtering, aggregation, and decision making to process the majority of events autonomously while escalating to the upper layer in exceptional or important cases\. That may help in such time\-critical domains as high\-frequency trading\.

Layers can be scaled independently, as [exemplified]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#distribution" >}}) by common web applications that comprise a highly scalable and resource\-consuming frontend, somewhat scalable backend and unscalable data layer\. Another example is an OS \(lower layer\) that runs multiple user applications \(upper layer\)\.

### Dependencies

Usually an upper layer depends on the *API* \(application programming interface\) of the layer directly below it\. That makes sense as the lower the layer is, the more stable it tends to be: a user\-facing software gets updated on a daily or weekly basis while hardware drivers may not change for years\. As every update of a component may destabilize other components that depend on it, it is much more preferable for a quickly evolving component to depend on others instead of the other way round\.

Some domains, including embedded systems and telecom, require their lower layers to be polymorphic as they deal with varied hardware or communication protocols\. In that case an upper layer \(e\.g\. OS kernel\) defines a *service provider interface* \(*SPI*\) which is implemented by every variant of the lower layer \(e\.g\. a device driver\)\. That allows for a single implementation of the upper layer to be interoperable with any subclass of the lower layer\. Such an approach enables [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}), [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}), and [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})\. 

There may also be an [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) layer between your system’s SPI and an external API\. It is called *Anticorruption Layer* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\], [*Database Abstraction Layer*](https://en.wikipedia.org/wiki/Database_abstraction_layer) / *Database Access Layer* \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] / *Data Mapper* \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\], *OS Abstraction Layer* or *Platform Abstraction Layer / Hardware Abstraction Layer*, depending on what kind of component it adapts\.

<figure>
<a href="/diagrams/Dependencies/Layers-1.png">
<picture>
<source srcset="/diagrams/Dependencies/Layers-1.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Layers-1.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Layers-1.png" alt="Individual layers may depend on other layers' APIs, SPIs, or both. In the last case the layer between the SPI and API is an adapter." loading="lazy" width="967" height="323" style="width:89%"/>
</picture>
</a>
</figure>

A layer can be *closed* \(*strict*\) or *open* \(*relaxed*\)\. A layer above a closed layer depends only on the closed layer right below it – it does not see through it\. Conversely, a layer above an open layer depends on both the open layer and the layer below it\. The open layer is transparent\. That helps keep a layer which encapsulates one or two subdomains small: if such a layer were closed, it would have to copy much of the interface of the layer below it just to pass the incoming requests which it does not handle through to the layer below\. The optimization of the open layer has a cost: the team that works on the layer above an open layer needs to learn two APIs which may have incompatible terminology\.

<figure>
<a href="/diagrams/Dependencies/Layers-2.png">
<picture>
<source srcset="/diagrams/Dependencies/Layers-2.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Layers-2.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Layers-2.png" alt="Dependencies for open and closed layers." loading="lazy" width="1143" height="302" style="width:93%"/>
</picture>
</a>
</figure>

If you ever need to *scale* \(run multiple instances of\) a layer, you may notice that a layer which sends requests naturally supports multiple instances, with the instance address being appended to each request so that its destination layer knows where to send the response\. On the other hand, if there are multiple instances of a layer you call into, you need a kind of [*Load Balancer*]({{< relref "../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) to dispatch requests among the instances\.

<figure>
<a href="/diagrams/Dependencies/Layers-3.png">
<picture>
<source srcset="/diagrams/Dependencies/Layers-3.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Layers-3.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Layers-3.png" alt="A load balancer helps access multiple instances of a layer directly below it." loading="lazy" width="823" height="363" style="width:93%"/>
</picture>
</a>
</figure>

### Applicability

*Layers* are <ins>good</ins> for:

- *Small and medium\-sized projects\.* Separating the business logic from the low\-level code should be enough to work comfortably on codebases below 100 000 lines in size\.
- *Specialized teams*\. You can have a team per layer: some people, who are proficient in optimization, work on the highly loaded infrastructure, while others talk to the customers and write the business logic\.
- *Deployment to a specific hardware\.* Frontend instances run on client devices, a backend needs much RAM, the data layer demands a large HDD and security\. There is no way to unite them into a single generic component\.
- *Flexible scaling\.* It is common to have hundreds or thousands of frontend instances being served by several backend processes that use a single database\.
- *Real\-time systems*\. Hardware components and network events often need the software to respond within a set time limit\. This is achievable by separating the time\-critical code from normal priority calculations\. See [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) and [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) for improved solutions\.


*Layers* are <ins>bad</ins> for:

- *Large projects\.* You are still going to enter *monolithic hell* \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] if you reach 1 000 000 lines of code\.
- *Low\-latency decision making*\. If your business logic needs to be applied in real time, you cannot tolerate the extra latency caused by the interlayer communication\.


### Relations

<figure>
<a href="/diagrams/Relations/Layers.png">
<picture>
<source srcset="/diagrams/Relations/Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Layers.png" alt="Splitting a layer into services and splitting a service into layers." loading="lazy" width="1063" height="462" style="width:100%"/>
</picture>
</a>
</figure>

*Layers*:

- Can be applied to the internals of any module, for example, layering [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) results in [*Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md" >}})\.
- Can be altered by [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) or extended with a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}), and/or [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) to form an extra layer\.
- Can be implemented by [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) yielding layers of services present in [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}), [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}), or [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.
- May be closely related to [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) or the derived [*Separated Presentation*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#upper-half-separated-presentation-open-host-service" >}})\.
- A layer often serves as a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}), and/or [*\(Shared\) Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\.


## Variants by isolation

There are [several grades]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}) of layer isolation between unstructured [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) and distributed *Tiers*\. All of them are widely used in practice: each step adds its specific benefits and drawbacks to those of the previous stages until at some point it makes more sense to reject the next deal because its cons are too inconvenient for you\.

### Synchronous layers, Layered Monolith

First you separate the high\-level logic from low\-level implementation details\. Then draw interfaces between them\. The layers will still call each other directly, but at least the code has been sorted out into some kind of structure, and you can now employ two or three dedicated teams, one per layer\. The cost is quite low – it is that the newly created interfaces will stand in the way of aggressive performance optimization\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| <span class="book-green">Structured code</span> | <span class="book-red">Lost opportunities for optimization</span> |
| <span class="book-green">Two or three teams</span> |  |

### Asynchronous layers

For the next step you may decide to take will be to isolate the layers’ execution threads and data\. The layers will communicate only through in\-process messages, which are slower than direct calls and harder to debug, but now each layer can run at its own pace – a must for interactive systems\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Structured code | <span class="book-red">No</span> opportunities for optimization |
| Two or three teams | <span class="book-red">Some troubles with debugging</span> |
| <span class="book-green">The layers may differ in latency</span> |  |

### A process per layer

Next, you may run each layer in a separate process\. You have to devise an efficient means of communication between them, but now the layers may differ in technologies, security, frequency of deployment, and even stability – the crash of one layer does not impact any of the others\. Moreover, you may scale each layer to make good use of the available CPU cores\. However, you will pay through even harder debugging, lower performance, and you will have to take care of error recovery, because if one of the components crashes, the others are likely to remain in an inconsistent state\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Structured code | No opportunities for optimization |
| Two or three teams | <span class="book-red">Troublesome</span> debugging |
| The layers may differ in latency | <span class="book-red">Some performance penalty</span> |
| <span class="book-green">The layers may differ in technologies</span> | <span class="book-red">Error recovery must be addressed</span> |
| <span class="book-green">The layers are deployed independently</span> |  |
| <span class="book-green">Software security isolation</span> |  |
| <span class="book-green">Software fault isolation</span> |  |
| <span class="book-green">Limited scalability</span> |  |

### Distributed tiers

Finally, you may separate the hardware which the processes run on – going all out for distribution\. This allows you to fine\-tune the system configuration, run parts of the system close to its clients, and physically isolate the most secure components, with your scalability limited only by your budget\. The price is paid in latency and debugging experience\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Structured code | No opportunities for optimization |
| Two or three teams | <span class="book-red">Even worse</span> debugging |
| The layers may differ in latency | <span class="book-red">Definite</span> performance penalty |
| The layers may differ in technologies | Error recovery must be addressed |
| The layers are deployed independently |  |
| <span class="book-green">Full</span> security isolation |  |
| <span class="book-green">Full</span> fault isolation |  |
| <span class="book-green">Full</span> scalability |  |
| <span class="book-green">Layers vary in hardware setup</span> |  |
| <span class="book-green">Deployment close to clients</span> |  |

## Variants of layer roles

Though the structure of every software system is unique, there is a common set of roles or functions that need to be covered by its code\. It is common wisdom that a piece of code should [*do one thing, and do it well*](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well), thus the code written to support the same kind of functionality tends to stick together and end up in a dedicated layer of a system\. This also helps your teams specialize and keeps their cognitive load low by limiting the amount of code they deal with, which allows for high productivity\.

That clarity of design, which separates technically different pieces, is opposed by a host of more pragmatic [*forces*]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#requirements-and-forces" >}}) that compel you to keep your code together:

- Performance can be easily optimized inside a component, while any communication between components will likely be much slower\.
- If many distinct workflows traverse a set of components, those components require large interfaces which take much effort to design, implement, and support\. You may end up spending more time maintaining your perfect architecture than writing the business logic which earns money for the company\.
- The more components you have, the harder it is to deploy them and keep them consistent, not to mention error recovery\. Moreover, as the number of system components increases, the big picture becomes elusive, and soon there is nobody who knows how to change the system if need arises\.


Balancing the [cohesers and decouplers]({{< relref "../analytics/the-heart-of-software-architecture/cohesers-and-decouplers.md" >}}) listed above results in coarse\-grained system components each of which covers several concerns, with some real\-world system compositions shown in the [Examples section]({{< relref "#examples" >}}) later in this chapter\. However, first we need to see which kinds of roles a system layer may incorporate:

<figure>
<a href="/diagrams/Variants/1/Layer%20Roles.png">
<picture>
<source srcset="/diagrams/Variants/1/Layer%20Roles.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Layer%20Roles.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Layer%20Roles.png" alt="A stack of layers: client or user, interface, application, domain, generic code, communication, data, and operating system and hardware." loading="lazy" width="883" height="664" style="width:100%"/>
</picture>
</a>
</figure>

### Interface \(API or UI\)

If a system serves a human user or remote client software, there is a part of it, called an *interface*, that deals with communication and translation between the system’s internal data model and one convenient for its clients\.

As an *interface* represents the system to its clients, it is a kind of [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) by definition \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\]\. If the client is another software system, the *interface* is called *Application Programming Interface* \(*API*\) and is likely to be implemented by a [*Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) which will receive a message through a well\-known protocol, check its correctness and authenticity of the sender, and forward the message’s payload to a layer below it\. In most cases it will also send response and notification messages by executing the converse tasks: translation from the system’s internal data format to something more convenient for its clients and sending the resulting message over a network protocol\.

When a system interacts with a human, it exposes another kind of *interface* – [*Human\-Machine Interface* \(*HMI*\) or *User Interface* \(*UI*\)]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}})\. The basics of its action are similar to the case of software\-to\-software interaction described above save that humans prefer visual or textual information instead of a highly structured Internet protocol\.

Another, less common kind of interface is called *Service Provider Interface* \(*SPI*\)\. It is declared by a system that relies on an external component and is implemented by that component’s authors to make it pluggable into the system\. *SPI*s are in use by [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) and [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) topologies, with device drivers being the best known example of pluggable components\.

Other kinds of *Proxies* adapt a system to foreign interfaces:

- An [*Anticorruption Layer* or *Open Host Service*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) translates between two software subsystems to loosen dependencies between them\.
- A [*Hardware Abstraction Layer* or *Operating System Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) stands between a system and an underlying hardware or OS, respectively, to make the system portable\.


<figure>
<a href="/diagrams/Variants/1/Interface%20-%20Kinds.png">
<picture>
<source srcset="/diagrams/Variants/1/Interface%20-%20Kinds.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Interface%20-%20Kinds.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Interface%20-%20Kinds.png" alt="A service wrapped with: a gateway with its API, a user interface, an Anticorruption Layer, a plugin and an Open Host Service with a Published Language." loading="lazy" width="1083" height="444" style="width:100%"/>
</picture>
</a>
</figure>

A *Proxy* that implements an *interface* may reside in a dedicated layer \(and there may be multiple *Proxies* stacked together, for example, a *Gateway* behind a [*Firewall*]({{< relref "../extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}})\) or be merged with a neighboring layer: for example, an [*API Gateway*]({{< relref "../extension-metapatterns/proxy.md#api-gateway" >}}) fills the roles of both *interface* and [*application*]({{< relref "#application-use-cases-or-integration" >}})\.

An interface layer can contain multiple components \(services, modules, or high\-level classes\) when the system below it supports several kinds of clients: a bank is likely to provide a web interface, a mobile application, and a SWIFT endpoint\. See [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) for a detailed description\.

<figure>
<a href="/diagrams/Variants/1/Interface%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Interface%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Interface%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Interface%20-%20Derived.png" alt="Diagrams of an API Gateway and Backends for Frontends." loading="lazy" width="1505" height="388" style="width:100%"/>
</picture>
</a>
</figure>

### Application \(use cases or integration\)

The *application* determines *what* a system does\. If it faces clients, the *application* runs client commands, called *use cases*, by executing chains of calls to its [*model*]({{< relref "#domain-business-rules-or-model" >}}) which knows *how* to do simple actions – the building blocks from which a use case consists\.

For example, to transfer money between accounts, the *application* asks the *model* to:

- Verify that the client is the owner of the account to be charged\.
- Calculate the bank’s fee for the transfer\.
- Subtract the amount to be transferred and the fee from the client’s account\.
- Add the fee to the bank’s account\.
- Tell the recipient’s bank to add the transferred amount to the target account\.


It is also responsible for dealing with any error that may occur in the process\. For example, if the target account does not exist or is blocked, the *application* will need to both refund the client and return a meaningful error message in the client’s preferred language\.

Other software, called [*control systems*]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}), is written to supervise hardware or software entities\. In that case there are no client requests or use cases – instead, the system reacts to signals from the components controlled\. It is the application layer which is responsible for the system’s behavior: if a smoke sensor detects fire, the application tells an alarm to sound\. This role is called *integration* – the system acts as a living organism, all its parts orderly moving in response to a stimulus perceived by a sense\.

When an application resides in a dedicated layer, it is called an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\. Like the [*interface*]({{< relref "#interface-api-or-ui" >}}) layer, the *application* layer may also contain multiple components: the bank will likely have distinct applications for its clients and for its managers\. The corresponding pattern is also called [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \(there is little distinction between [*Proxies*]({{< relref "../extension-metapatterns/proxy.md" >}}) and *Orchestrators* in that topology\)\.

Some systems lack the *application* role – they are structured as [*Pipelines*]({{< relref "../basic-metapatterns/pipeline.md" >}}), so that whatever enters one end of the system passes through all its components and pops out, digested, at the end\. In that case it is the very structure of the system – the connections between its components – that drive the order of events\.

<figure>
<a href="/diagrams/Variants/1/Application%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Application%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Application%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Application%20-%20Derived.png" alt="Backends for Frontends between a gateway and a monolithic service; a pipeline with use case logic hardwired into the graph of connections." loading="lazy" width="1323" height="403" style="width:100%"/>
</picture>
</a>
</figure>

### Domain \(business rules or model\)

The *domain* layer *models* the real\-world system which your software operates or emulates\. It contains rules that describe *how* to do anything that your clients may want or *how* the hardware which you control is expected to operate\.

Back to the banking example, the *domain* layer can:

- Find if an account is valid and who owns it\.
- Calculate a fee\.
- Add money to or subtract it from an account\.
- Transfer money to another bank\.


And it has some tricks up its sleeves to assure that nothing it does about money is ever lost, even if the network is disconnected or the hardware it runs on fails\.

In most cases the *domain* layer is the largest one and it is the one which makes your software valuable for business because it actually *does* whatever your software is about\. It is also [the only layer in which OOP classes may model real\-world entities](http://tedfelix.com/software/jacobson1992.html)\.

As the largest layer, the *domain* is the first among them to be subdivided:

- The most common way is to partition it into *subdomains* – loosely coupled subsets of your system’s functionality – yielding [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) \(when other layers are fragmented along the same lines or replicated among the subdomain components\) or a [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}})\.
- Rare cases allow for [*hierarchical* decomposition]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) where most components blend [*application*]({{< relref "#application-use-cases-or-integration" >}}) and *domain* roles\.
- Last but not least, we can use separate *models* for making changes \(executing *commands*\) and for analytics \(running *queries*\), giving rise to [*Command Query Responsibility Segregation* \(*CQRS*\)]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}})\. This makes sense because a command usually involves many fields of a single record \(database row\) while a query runs over select rows of all the records – they vary in how they access and treat the data\. It is common to have a record wrapped into an OOP class in the command model, while the query model, if it is not omitted completely, provides for direct access to the database\.


<figure>
<a href="/diagrams/Variants/1/Domain%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Domain%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Domain%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Domain%20-%20Derived.png" alt="Diagrams of Services, Sandwich, Hierarchy, and Command-Query Responsibility Segregation." loading="lazy" width="863" height="783" style="width:74%"/>
</picture>
</a>
</figure>

### Generic code \(libraries and utilities\)

*Generic code* is something unrelated to your business but still used by your logic: a graph traversal algorithm, an e\-mail server, or even a computer vision framework to identify duplicate user avatars\.

In most cases *generic code* stays together with the [*domain*\-level code]({{< relref "#domain-business-rules-or-model" >}}) which calls it\. However, when the *domain* layer gets subdivided into services, there emerge [multiple options]({{< relref "../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}):

- If the *generic code* is not shared, it moves into the service that uses it\. See [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- If it needs to be shared, it can be:
  - Extracted into a dedicated service, as in [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\.
  - Replicated as a [*Sidecar*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) attached to every instance of each service that uses it\.
  - Copied into the codebases of the services to allow each team to change it independently from other teams – see *Separate Ways* in \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\.


<figure>
<a href="/diagrams/Variants/1/Generic%20Code%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Generic%20Code%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Generic%20Code%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Generic%20Code%20-%20Derived.png" alt="Diagrams of Services, Service-Oriented Architecture, and Microservices with sidecars, with components that carry generic code highlighted." loading="lazy" width="1363" height="483" style="width:100%"/>
</picture>
</a>
</figure>

### Communication \(middleware\)

If your system is comprised of multiple components, they need a way to communicate, which may be as simple as in\-process method calls or as complex as a [distributed consensus protocol](https://en.wikipedia.org/wiki/Paxos_(computer_science))\. The *communication infrastructure*, when used consistently throughout a system, makes a distinct virtual \(conjoining separately deployed nodes\) system layer, called [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}})\.

<figure>
<a href="/diagrams/Variants/1/Communication%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Communication%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Communication%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Communication%20-%20Derived.png" alt="Multiple instances of a communication library represented as a virtual middleware layer." loading="lazy" width="1343" height="324" style="width:100%"/>
</picture>
</a>
</figure>

### Data \(persistence\)

Most systems but the simplest [*Pipelines*]({{< relref "../basic-metapatterns/pipeline.md" >}}) are stateful – they remember something about their users or their environment:

- A backend would usually *persist* useful facts to a *database* which makes a dedicated *persistence layer*\.
- A [real\-time *control system*]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) does not have the leisure to access anything remote, therefore its state is embedded in\-memory into its [*domain* layer]({{< relref "#domain-business-rules-or-model" >}})\.
- Complex distributed frameworks that implement [*Actors*]({{< relref "../basic-metapatterns/services.md#actors" >}}) or [*Space\-Based Architecture*]({{< relref "../implementation-metapatterns/mesh.md#space-based-architecture" >}}) both keep each service’s data inside the service’s memory for fast access and back all the changes to a persistent datastore to support failure recovery\.


<figure>
<a href="/diagrams/Variants/1/Data%20-%20Derived.png">
<picture>
<source srcset="/diagrams/Variants/1/Data%20-%20Derived.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Data%20-%20Derived.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Data%20-%20Derived.png" alt="Diagrams of a three-tier system, hierarchical control system, and Space-Based Architecture." loading="lazy" width="1063" height="449" style="width:100%"/>
</picture>
</a>
</figure>

If the *persistence layer* becomes a system’s performance bottleneck, as it often does, one of [the cures]({{< relref "../extension-metapatterns/shared-repository.md#evolutions" >}}) is using several specialized datastores, leading to [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})\.

<figure>
<a href="/diagrams/Variants/1/Data%20-%20Evolutions.png">
<picture>
<source srcset="/diagrams/Variants/1/Data%20-%20Evolutions.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Data%20-%20Evolutions.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Data%20-%20Evolutions.png" alt="A load-balanced service over a database evolves into a monolith with two specialized databases or into a load-balanced stateless service over database replicas with a single leader." loading="lazy" width="1162" height="523" style="width:100%"/>
</picture>
</a>
</figure>

### Operating system and hardware

Your software always runs on *hardware* and usually relies on an *operating system* \(*OS*\) for file and network access and memory management\. Even though many modern backends don’t care about such low\-level details and omit them on their diagrams, embedded or system software communicates with its *OS* or *hardware* directly, making the corresponding components indispensable parts of its topology\.

Integrating multiple pieces of *hardware* into an intelligently behaving system is usually what [*control software*]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) is written for\. Such systems [often follow]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#variants" >}}) the [*Pedestal*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#pedestal" >}}), [*Actors*]({{< relref "../basic-metapatterns/services.md#class-like-actors" >}}), or [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) architectures\.

<figure>
<a href="/diagrams/4Kinds/Control%20-%20variants.png">
<picture>
<source srcset="/diagrams/4Kinds/Control%20-%20variants.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/4Kinds/Control%20-%20variants.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/4Kinds/Control%20-%20variants.png" alt="Diagrams of control systems with the following architectures: monolithic, actors, Pedestal, hierarchical." loading="lazy" width="1224" height="664" style="width:100%"/>
</picture>
</a>
</figure>

## Examples

The notion of layering seems to be so natural to our minds that most known architectures are layered\. Not surprisingly, there are several approaches to assigning functionality to and naming the layers:

- [*ECB*]({{< relref "#entity-control-boundary-ecb-entity-boundary-control-ebc-boundary-control-entity-bce" >}}) distinguishes a client\-facing layer, use cases and domain logic\.
- [*DDD*]({{< relref "#domain-driven-design-ddd-layers" >}}) adds the infrastructure layer\.
- [*Tiers*]({{< relref "#three-tier-architecture" >}}) are distributed *Layers* that usually include frontend, backend and database\.
- [Layering of an embedded system]({{< relref "#embedded-systems" >}}) often matches its supply chain\. 


### Entity\-Control\-Boundary \(ECB\), Entity\-Boundary\-Control \(EBC\), Boundary\-Control\-Entity \(BCE\)

<figure>
<a href="/diagrams/Variants/1/ECB.png">
<picture>
<source srcset="/diagrams/Variants/1/ECB.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/ECB.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/ECB.png" alt="The boundary, control and entity layers." loading="lazy" width="903" height="383" style="width:100%"/>
</picture>
</a>
</figure>

[*Entity\-Control\-Boundary*](https://en.wikipedia.org/wiki/Entity%E2%80%93control%E2%80%93boundary) \(*ECB*\) or other combinations of these words \(*EBC* and *BCE*\) designate a system composed of the following layers:

- *Boundary* – the layer which [interacts with the system’s *clients* or *users*]({{< relref "#interface-api-or-ui" >}})\. See [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}})\.
- *Control* – the layer which [contains *use cases*]({{< relref "#application-use-cases-or-integration" >}}) – sequences of actions on the system’s internals that should be made to process a client’s request or respond to a user’s action\. See [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\.
- *Entity* – the bulk of the system’s business logic and data\.


A closer look at an *ECB* system may reveal a finer\-grained structure that resembles [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) or [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) as each layer is composed of modules or objects:

<figure>
<a href="/diagrams/Variants/1/ECB%20as%20SOA.png">
<picture>
<source srcset="/diagrams/Variants/1/ECB%20as%20SOA.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/ECB%20as%20SOA.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/ECB%20as%20SOA.png" alt="The boundary, control and entity layers, each subdivided into several services." loading="lazy" width="866" height="483" style="width:100%"/>
</picture>
</a>
</figure>

### Domain\-Driven Design \(DDD\) Layers

<figure>
<a href="/diagrams/Variants/1/DDD.png">
<picture>
<source srcset="/diagrams/Variants/1/DDD.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/DDD.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/DDD.png" alt="The four layers of Domain-Driven Design: presentation, application, domain, and infrastructure." loading="lazy" width="743" height="476" style="width:86%"/>
</picture>
</a>
</figure>

\[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\], a methodology for enterprise\-scale backend development, extends the more generic [*Entity\-Control\-Boundary*]({{< relref "#entity-control-boundary-ecb-entity-boundary-control-ebc-boundary-control-entity-bce" >}}) with a new *Infrastructure* layer responsible for [*communication*]({{< relref "#communication-middleware" >}}) and [*persistence*]({{< relref "#data-persistence" >}}) roles which don’t exist in most desktop applications\. Its layers are called:

- *Presentation* \([*User Interface*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}})\) – the user\-facing component \(frontend, UI\)\. It should be highly responsive to the user's input\. See [*Separated Presentation*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#upper-half-separated-presentation-open-host-service" >}})\.
- *Application* \([*Integration*]({{< relref "#application-use-cases-or-integration" >}}), *Service*\) – the high\-level scenarios which build upon the API of the *domain* layer\. It should be easy to change and to deploy\. See [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\.
- *Domain* \([*Model*]({{< relref "#domain-business-rules-or-model" >}}), *Business Rules*\) – the bulk of the mid\- and low\-level business logic\. It should usually be well\-tested and performant\.
- *Infrastructure* \(*Utility*, *Data Access*\) – the utility components devoid of business logic\. Their stability and performance is business\-critical but updates to their code are rare\.


For example, an online banking system comprises:

- the presentation layer which is its frontend; 
- the application layer which implements sequences of steps for payment, card to card transfer, and viewing a client’s history of transactions;
- the domain layer contains classes for various kinds of cards and accounts;
- the infrastructure layer with a database and libraries for encryption and interbank communication\.


However in practice you are much more likely to encounter the derived [*DDD\-style Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#ddd-style-hexagonal-architecture-onion-architecture-clean-architecture" >}}) than the original *DDD Layers*\.

### Three\-Tier Architecture

<figure>
<a href="/diagrams/Variants/1/Three-Tier.png">
<picture>
<source srcset="/diagrams/Variants/1/Three-Tier.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Three-Tier.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Three-Tier.png" alt="Four instances of the presentation layer accessing two instances of the logic layer accessing a single database." loading="lazy" width="760" height="394" style="width:100%"/>
</picture>
</a>
</figure>

Here the focus lies with the distribution of the components over heterogeneous hardware \(*Tiers*\):

- *Presentation* \([*Frontend*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}})\) tier – a user\-facing application which runs on a user’s hardware\. It is very scalable and responsive, but insecure\.
- *Logic* \(*Backend*\) tier – the business logic which is deployed on the service provider’s side\. Its scalability is limited mostly by the funding committed, security is good but latency is high\.
- *Data* \(*Database*\) tier – a service provider’s database which runs on a dedicated server\. It is not scalable but is very secure\.


In this case the division into layers resolves the conflict between scalability, latency, security, and cost as discussed in detail in the [chapter on distribution]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md#distribution" >}})\.

<aside>

> *Tiers* don’t map to *Layers* directly\. For example, a protocol support library which is used for communication between services belongs to the lowest \(infrastructure\) layer but to the middle \(backend\) tier\. The discrepancy is rooted in different natures \([views of the *4\+1 model*](https://en.wikipedia.org/wiki/4%2B1_architectural_view_model)\) of the patterns in question: *Layers* show the logical composition of the system while *Tiers* deal with its physical structure \(deployment\)\.

</aside>

### Embedded systems

<figure>
<a href="/diagrams/Variants/1/Embedded.png">
<picture>
<source srcset="/diagrams/Variants/1/Embedded.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/1/Embedded.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/1/Embedded.png" alt="An embedded system with the following pairs of layers: user interface and human-machine interface, software development kit and hardware abstraction layer, firmware and hardware." loading="lazy" width="763" height="373" style="width:100%"/>
</picture>
</a>
</figure>

Bare metal and micro\-OS systems which run on low\-end chips use a different terminology, which is not unified across domains\. A generic example involves:

- *Presentation* – a UI engine used by the *HMI*\. It may be a third\-party library or come as a part of the *SDK*\.
- [*Human\-Machine Interface*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) \(*HMI* aka *MMI*\) – the UI and high\-level business logic for user scenarios, written by a [value\-added reseller](https://en.wikipedia.org/wiki/Value-added_reseller)\.
- *Software Development Kit* \(*SDK*\) – the mid\-level business logic and device drivers, written by the [original equipment manufacturer](https://en.wikipedia.org/wiki/Original_equipment_manufacturer)\.
- [*Hardware Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(*HAL*\) – the low\-level code that abstracts hardware registers to enable code reuse between hardware platforms\.
- *Firmware of Hardware Components* – usually closed\-source binary pre\-programmed into chips by chipmakers\.
- *Hardware* itself\.


It is of note that in this approach the layers form strongly coupled pairs\. Each pair is implemented by a separate party of the supply chain, which is an extra force that shapes the system into layers\.

An example of such a system can be found in an old mobile phone or a digital camera\.

## Evolutions

Layers are not without drawbacks which may force your system to evolve\. A summary of such evolutions is given below while more details can be found in [Appendix E]({{< relref "../appendices/evolutions-of-architectures/_index.md" >}})\.

### [Evolutions that make more layers]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-layers-that-make-more-layers.md" >}})

Not all the layered architectures are equally layered\. A [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) with a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) or database has already stepped into the realm of *Layers* but is far from reaping all its benefits\. Such a system may continue its course in a few ways that were previously [discussed for *Monolith*]({{< relref "../basic-metapatterns/monolith.md#evolutions-to-layers" >}}):

- Employing a *database* \(if you don’t use one\) lets you rely on a thoroughly optimized state\-of\-the\-art subsystem for data processing and storage\.
- [*Proxies*]({{< relref "../extension-metapatterns/proxy.md" >}}) are similarly reusable generic modules to be added at will\.
- Implementing an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) on top of your system may improve programming experience and runtime performance for your clients\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Layers.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Layers.png" alt="A diagram of calls in a layered system. A single request from a client is translated by an Orchestrator into multiple calls to lower layers." loading="lazy" width="943" height="424" style="width:100%"/>
</picture>
</a>
</figure>

It is also common to:

- Have the business logic divided into two layers\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.png" alt="A backend is subdivided into application and domain layers." loading="lazy" width="1027" height="243" style="width:100%"/>
</picture>
</a>
</figure>

### [Evolutions that help large projects]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-layers-that-help-large-projects.md" >}})

The main drawback \(and benefit as well\) of *Layers* is that much or all of the business logic is kept together in one or two components\. That allows for easy debugging and fast development in the initial stages of the project but slows down and complicates work as the project grows in size \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\]\. The only way for a growing project to survive and continue evolving at a reasonable speed is to divide its business logic into several smaller, [thus less complex]({{< relref "../foundations-of-software-architecture/modules-and-complexity.md" >}}), components that match subdomains \(*bounded contexts* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\)\. There are several options for such a change, with their applicability depending on the domain:

- In a [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}) the middle layer with the main business logic is divided into [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) leaving the upper [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and lower [*database*]({{< relref "../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) layers intact for future evolutions\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20Domain%20to%20Services.png" alt="The domain layer is split into subdomain components, making a Sandwich." loading="lazy" width="1083" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- Sometimes the business logic can be represented as a set of directed graphs which is known as [*Event\-Driven Architecture*]({{< relref "../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}})\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20to%20Event-Driven%20Architecture.png" alt="A backend is subdivided into a pipeline." loading="lazy" width="1166" height="264" style="width:100%"/>
</picture>
</a>
</figure>

- If you are lucky, your domain makes a [*Top\-Down Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Hierarchy.png" alt="The lower layers of a system are subdivided, resulting in a hierarchy." loading="lazy" width="1103" height="249" style="width:100%"/>
</picture>
</a>
</figure>

### [Evolutions that improve performance]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-layers-to-improve-performance.md" >}})

There are several ways to improve the performance of a layered system\. One we have [already discussed for *Shards*]({{< relref "../basic-metapatterns/shards.md#evolutions-that-share-data" >}}):

- [*Space\-Based Architecture*]({{< relref "../implementation-metapatterns/mesh.md#space-based-architecture" >}}) co\-locates the database and business logic and scales both dynamically\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Space-Based%20Architecture.png" alt="The database is migrated to a Data Grid, resulting in a scalable Space-Based Architecture." loading="lazy" width="1051" height="264" style="width:100%"/>
</picture>
</a>
</figure>

Others are new:

- Merging several layers improves latency by eliminating the communication overhead\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Merge.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Merge.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20Merge.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20Merge.png" alt="The application and domain layers are merged." loading="lazy" width="1023" height="243" style="width:100%"/>
</picture>
</a>
</figure>

- [Scaling]({{< relref "../basic-metapatterns/shards.md" >}}) some of the layers may improve throughput\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers_%20Shard.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers_%20Shard.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers_%20Shard.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers_%20Shard.png" alt="The application and domain layers are independently sharded." loading="lazy" width="983" height="305" style="width:100%"/>
</picture>
</a>
</figure>

- [*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) is the name for using multiple specialized databases\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Polyglot%20Persistence.png" alt="The database layer is subdivided into specialized databases, resulting in Polyglot Persistence." loading="lazy" width="1023" height="248" style="width:100%"/>
</picture>
</a>
</figure>

### [Evolutions to gain flexibility]({{< relref "../appendices/evolutions-of-architectures/evolutions-of-layers-to-gain-flexibility.md" >}})

The last group of evolutions to consider is about making the system more adaptable\. We have already discussed the following [evolutions for *Monolith*]({{< relref "../basic-metapatterns/monolith.md#evolutions-with-plugins" >}}):

- The behavior of the system may be modified with [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}})\.
- [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) allows for abstracting the business logic from the technologies used in the project\.
- [*Scripts*]({{< relref "../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) allow for customization of the system’s logic on a per client basis\.


<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Layers%20-%20Further%202.png" alt="Diagrams of Layers with plugins, Layers with scripts, and Hexagonal Architecture with a layered core." loading="lazy" width="983" height="503" style="width:100%"/>
</picture>
</a>
</figure>

There is one new evolution which modifies the upper \(*orchestration*\) layer:

- The [orchestration layer]({{< relref "../extension-metapatterns/orchestrator.md" >}}) may be split into [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) to match the individual needs of several kinds of clients\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.png">
<picture>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Backends%20for%20Frontends.png" alt="The application layer is split into Backends for Frontends." loading="lazy" width="1003" height="323" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

*Layered architecture* separates the high\-level logic from the low\-level details\. It is superior for medium\-sized projects as it supports rapid development by two or three teams, is flexible enough to resolve conflicting forces, and provides many options for further evolution, which will come in handy when the project grows in size and complexity\.