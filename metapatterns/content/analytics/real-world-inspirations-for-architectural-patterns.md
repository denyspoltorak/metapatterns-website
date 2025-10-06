+++
weight = 4
title = "Real-world inspirations for architectural patterns"
description = "Styles and patterns of software architecture have real-world inspirations and parallels. Learning about them may help us to invent new patterns when needed."
[sitemap]
  priority = 0.5
+++

# Real\-world inspirations for architectural patterns

As architectural patterns are usually technology\-independent, they must mostly be shaped by the foundational principles of software engineering\. And because the same principles are likely at work at every level of a software system, we may expect similar structures to appear on many levels of software, given similar circumstances – which is not always attainable, for the system\-wide scope \(which means that there are multiple clients and libraries\) and distributed nature \(which deals with faults of individual components\) of many patterns of system architecture don’t have direct counterparts in smaller single\-process software\. Thus we expect to observe a fractal nature for the more generic patterns while narrowly specialized ones are present at only one or two scopes of software design\.

Another thought to consider is that it’s not in human nature to invent something new – we are much more adept in imitating and combining whatever we see around us\. That is why it’s so hard to find a genuine xenopsychology in literature or movies – to the extent that the eponymous Alien is just an overgrown [parasitoid wasp](https://en.wikipedia.org/wiki/Parasitoid_wasp)\. Hence there is another pathway to pursue – identifying the patterns which we know from software engineering in the world around us, as the authors of \[[POSA2]({{< relref "../appendices/books-referenced.md#posa2" >}})\] did decades ago\.

Let’s go\!

## Basic metapatterns

The basic patterns lay the foundation for any system by paving ways to *divide* it into components to *conquer* its [complexity]({{< relref "../foundations-of-software-architecture/modules-and-complexity.md" >}})\. We are going to observe them all around:

### Monolith

<figure>
<a href="/diagrams/Contents/Monolith.png">
<picture>
<source srcset="/diagrams/Contents/Monolith.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Monolith.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Monolith.png" alt="Monolith" loading="lazy" width="1003" height="243" style="width:100%"/>
</picture>
</a>
</figure>

[*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) means encapsulation – we use the thing without looking inside:

- You interact with your dog \(or your smartphone\) through their interface without thinking of their internals\.
- A function exposes its name, arguments and, probably, some comments\. The implementation is hidden from its users\.
- An object has a list of public methods\.
- A module or a library exports several functions for use by its clients\.
- A program is configured through its command line parameters and managed through its [CLI](https://en.wikipedia.org/wiki/Command-line_interface)\. We don’t care how the Linux utilities \(e\.g\. *top* or *cat*\) work – we just run them\.
- A whole distributed system may be hidden behind a web page in your browser – and you never imagine its complexity unless you have worked on something of a kind\.


### Shards

<figure>
<a href="/diagrams/Contents/Shards.png">
<picture>
<source srcset="/diagrams/Contents/Shards.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Shards.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Shards.png" alt="Shards" loading="lazy" width="783" height="243" style="width:100%"/>
</picture>
</a>
</figure>

[*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}) is about having multiple instances of something, which often differ in their data:

- A company employs many programmers to accelerate development of its projects\.
- Carrying two mobile phones from different operators fits this pattern as well\.
- This is how they make modern processors more powerful: by adding more cores, not by running them faster\.
- Objects in OOP are the perfect example of having multiple instances that vary in their data\.
- Running several shells in Linux is a kind of sharding\.
- A client application of a multi\-user online game is a shard\.


### Layers

<figure>
<a href="/diagrams/Contents/Layers.png">
<picture>
<source srcset="/diagrams/Contents/Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Layers.png" alt="Layers" loading="lazy" width="783" height="243" style="width:95%"/>
</picture>
</a>
</figure>

[*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) is the separation of responsibilities between external and internal components:

- In winter we wear soft clothes on our body, a warm sweater over them, and a wind\-proof jacket as the external layer\.
- An object comprises high\-level public methods, low\-level privates, and data\.
- An OS has a UI which runs over user\-space software over an OS kernel over device drivers over the hardware\.
- Your web browser executes a frontend which communicates to a backend which uses a database\.


### Services

<figure>
<a href="/diagrams/Contents/Services.png">
<picture>
<source srcset="/diagrams/Contents/Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Services.png" alt="Services" loading="lazy" width="943" height="246" style="width:95%"/>
</picture>
</a>
</figure>

[*Services*]({{< relref "../basic-metapatterns/services.md" >}}) boil down to composition and [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns):

- We have legs, arms, and other specialized members\.
- A gadget contains specialized chips for activities it supports\.
- \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] advocates for an object to incorporate smaller objects \(composition over inheritance\)\.
- Applications often delegate parts of their logic to specialized modules or libraries\.
- An OS dedicates a driver for each piece of hardware installed\. Moreover, it provides many tools to its users – instead of tackling all the user needs within the kernel\.
- \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] describes the way to subdivide a large system into loosely coupled components\.


### Pipeline

<figure>
<a href="/diagrams/Contents/Pipeline.png">
<picture>
<source srcset="/diagrams/Contents/Pipeline.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Pipeline.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Pipeline.png" alt="Pipeline" loading="lazy" width="1003" height="203" style="width:95%"/>
</picture>
</a>
</figure>

[*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}) is about the stepwise transformation of data:

- The pattern got its name from real\-world plumbing\.
- You’ll see similar arrangements in [cellular metabolism](https://en.wikipedia.org/wiki/Metabolism)\.
- It is the basis for [functional programming]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#functional-decentralized-streaming-paradigm--choreography" >}})\.
- Linux command line tools are often skillfully composed into pipelines\.
- Hardware is full of pipelines: from [CPU](https://en.wikipedia.org/wiki/Instruction_pipelining) and [GPU](https://en.wikipedia.org/wiki/Graphics_pipeline) to audio and video processing\.
- Finally, a UI wizard passes its users through a series of screens\.


## Extension metapatterns

An extension pattern encapsulates one or two aspects of the system’s implementation\. It may appear only on design levels which have those particular aspects:

### Middleware

<figure>
<a href="/diagrams/Contents/Middleware.png">
<picture>
<source srcset="/diagrams/Contents/Middleware.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Middleware.png" alt="Middleware" loading="lazy" width="1003" height="263" style="width:100%"/>
</picture>
</a>
</figure>

A [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) abstracts scaling and/or intercommunication:

- The network of post offices is a middleware – you push the letter into a mailbox and it automagically appears at its destination’s door\.
- A [bus depot](https://en.wikipedia.org/wiki/Bus_depot) may mean a bus garage which deploys as many buses as needed to service the traffic or a bus station where people come to have a ride, regardless of the exact vehicle model they’ll take\.
- Hardware is full of another kind of [buses](https://en.wikipedia.org/wiki/Bus_(computing)) that unify means of communication\.
- TCP and UDP sockets hide the details of the underlying network\.
- A distributed [actor framework]({{< relref "../basic-metapatterns/services.md#actors" >}}) allows an actor to address another actor without knowing where it is deployed\.


### Shared Repository

<figure>
<a href="/diagrams/Contents/Shared%20Repository.png">
<picture>
<source srcset="/diagrams/Contents/Shared%20Repository.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Shared%20Repository.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Shared%20Repository.png" alt="Shared Repository" loading="lazy" width="963" height="243" style="width:100%"/>
</picture>
</a>
</figure>

A [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}}) provides data storage and/or data change notifications:

- Everybody in the room may use a ~black~whiteboard to express and exchange their ideas\.
- An Internet forum works in a similar way – people post their arguments there for others to see them and get notified on answers\.
- RAM and CPU caches are kinds of shared repositories\. CPU caches are [kept synchronized through notifications](https://en.wikipedia.org/wiki/Cache_coherency_protocols_(examples))\.
- *Observer* \[[GoF](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.biwhq98sqmss)\] is about getting notified when a shared object changes\.
- Services or service instances may share a database\.


### Proxy

<figure>
<a href="/diagrams/Contents/Proxy.png">
<picture>
<source srcset="/diagrams/Contents/Proxy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Proxy.png" alt="Proxy" loading="lazy" width="1023" height="363" style="width:100%"/>
</picture>
</a>
</figure>

A [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}) isolates a system from its environment by translating between internal and external protocols and/or implementing generic aspects of communication:

- You may need a translator to understand foreign people or have a secretary to deal with routine tasks\. A local guide combines both roles\.
- An adapter makes several hardware plugs \(or software frameworks\) compatible\.
- Your Wi\-Fi router is a proxy between your laptop and the Internet\.
- A compiler is a kind of proxy between source code and bytecode\.


### Orchestrator

<figure>
<a href="/diagrams/Contents/Orchestrator.png">
<picture>
<source srcset="/diagrams/Contents/Orchestrator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Orchestrator.png" alt="Orchestrator" loading="lazy" width="1023" height="263" style="width:100%"/>
</picture>
</a>
</figure>

An [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) integrates several components by implementing high\-level use cases and/or keeping them in sync:

- A taxi driver orchestrates his car’s internals\.
- A *Facade* \[[GoF](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.biwhq98sqmss)\] provides a high\-level interface for a system while a *Mediator* \[[GoF](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.biwhq98sqmss)\] integrates a system by spreading changes initiated by the system’s components\.
- A linker composes a working program out of disjunct modules\.


## Fragmented metapatterns

A fragmented pattern uses small specialized components to approach a case which is hard to resolve with more generic means\. The high degree of specialization leads to even fewer examples:

### Polyglot Persistence

<figure>
<a href="/diagrams/Contents/Polyglot%20Persistence.png">
<picture>
<source srcset="/diagrams/Contents/Polyglot%20Persistence.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Polyglot%20Persistence.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Polyglot%20Persistence.png" alt="Polyglot Persistence" loading="lazy" width="1023" height="303" style="width:100%"/>
</picture>
</a>
</figure>

[*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) is about having multiple containers for data:

- A warehouse or a cargo ship has dedicated storage areas with extra facilities for combustible, toxic, and frozen goods\.
- A computer has CPU caches, RAM, flash, and hard drives for temporary or permanent data storage\.
- There are map, list, and array – each with its pros and cons\. A large class would often use two or three kinds of containers and not without reason\.


### Backends for Frontends

<figure>
<a href="/diagrams/Contents/Backends%20for%20Frontends.png">
<picture>
<source srcset="/diagrams/Contents/Backends%20for%20Frontends.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Backends%20for%20Frontends.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Backends%20for%20Frontends.png" alt="Backends for Frontends" loading="lazy" width="1003" height="383" style="width:100%"/>
</picture>
</a>
</figure>

[*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) is about treating different kinds of clients individually:

- A bank is likely to reserve a couple of employees to serve rich clients\.
- A Wi\-Fi router has many management interfaces: web, mobile application, CLI, and probably [TR\-069](https://en.wikipedia.org/wiki/TR-069)\.
- A multiplayer game may provide desktop and mobile client applications\.


### Service\-Oriented Architecture

<figure>
<a href="/diagrams/Contents/Service-Oriented%20Architecture.png">
<picture>
<source srcset="/diagrams/Contents/Service-Oriented%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Service-Oriented%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Service-Oriented%20Architecture.png" alt="Service-Oriented Architecture" loading="lazy" width="983" height="401" style="width:100%"/>
</picture>
</a>
</figure>

[*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) applies OOP techniques, including component reuse, to deal with complex systems:

- That’s what you have inside your car\. Many of its internals rely on the car’s battery for power supply instead of having a small battery installed inside every component\.
- Cities are built in the same way – schools, markets, and railways serve multiple houses\.
- It’s the same with user space of operating systems: there is a shared UI framework which interfaces with as\-many\-as\-needed applications, each of which calls shared libraries \(DLLs\)\.


### Hierarchy

<figure>
<a href="/diagrams/Contents/Hierarchy.png">
<picture>
<source srcset="/diagrams/Contents/Hierarchy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Hierarchy.png" alt="Hierarchy" loading="lazy" width="907" height="303" style="width:100%"/>
</picture>
</a>
</figure>

[*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) distributes system’s complexity over multiple levels:

- This is how large companies and armies are managed\.
- Large projects are made of services which contain modules which contain classes which contain methods\.


## Implementation metapatterns

An implementation pattern highlights the peculiar internal arrangements of a component\. Such patterns are deeply specialized:

### Plugins

<figure>
<a href="/diagrams/Contents/Plugins.png">
<picture>
<source srcset="/diagrams/Contents/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Plugins.png" alt="Plugins" loading="lazy" width="843" height="403" style="width:100%"/>
</picture>
</a>
</figure>

[*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) make a component’s behavior flexible through delegating its parts to small external additions:

- This is how we use tools for our work – a man becomes a digger when given a shovel\.
- *Strategy* \[[GoF](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.biwhq98sqmss)\] is the thing\.


### Hexagonal Architecture

<figure>
<a href="/diagrams/Contents/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Contents/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="980" height="363" style="width:100%"/>
</picture>
</a>
</figure>

[*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) protects the internals of a system from its environment:

- A drill or a screwdriver has replaceable bits\.
- *OS Abstraction Layer* and *Hardware Abstraction Layer* in embedded systems or *Anti\-Corruption Layer* in \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] are all about that\.


### Microkernel

<figure>
<a href="/diagrams/Contents/Microkernel.png">
<picture>
<source srcset="/diagrams/Contents/Microkernel.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Microkernel.png" alt="Microkernel" loading="lazy" width="1031" height="304" style="width:100%"/>
</picture>
</a>
</figure>

[*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) shares the goods of resource providers among resource users:

- It’s like a bank that takes money from the rich to distribute them among the poor\.
- This is what an OS is for\. Its scheduler shares the CPU, the memory subsystem shares RAM, while the device drivers provide access to the peripherals\.
- Cloud services are based on sharing computation resources among clients\.


### Mesh

<figure>
<a href="/diagrams/Contents/Mesh.png">
<picture>
<source srcset="/diagrams/Contents/Mesh.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Mesh.png" alt="Mesh" loading="lazy" width="863" height="261" style="width:100%"/>
</picture>
</a>
</figure>

[*Mesh*]({{< relref "../implementation-metapatterns/mesh.md" >}}) is like grassroots movements – self\-organizing and survival through redundancy:

- Ants and bees are small, autonomous, and efficient\. Their strength comes from their numbers\.
- Road networks and power grids don’t collapse if some of their components are damaged as they are highly redundant\.
- Torrents, mobile communications, and the Internet infrastructure are known for their robustness\.


## Summary

Architectural patterns have parallels in the natural world, our society and/or different levels of computer hardware and software\. Learning about them helps us feel the driving forces behind patterns and be more flexible and creative in using the patterns we know and devising new ones\.