+++
weight = 5
title = "Plugins"
description = "This chapter explores the highly customizable Plugins architecture and its subtypes: Plugin, Ambassador Plugin, Extension, Addin, and Addon."
images = ["/diagrams/Web/og/Plugins.png"]
[sitemap]
  priority = 0.8
+++

# Plugins {anchor=false}

<figure>
<a href="/diagrams/Main/Plugins.png">
<picture>
<source srcset="/diagrams/Main/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Plugins.png" alt="A diagram for Plugins Architecture, in abstractness-subdomain-sharding coordinates." loading="lazy" width="942" height="554" style="width:100%"/>
</picture>
</a>
</figure>

*Overspecialize, and you breed in weakness\.* Customize the system through attachable modules\.

<ins>Known as:</ins> Plug\-In Architecture \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], [Extension Architecture](https://www.uber.com/en-UA/blog/microservice-architecture/), \(misapplied\) Microkernel \(Architecture\) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], Plugin \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\], [Strategy](https://refactoring.guru/design-patterns/strategy) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\], Reflection \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\], Aspects, Hooks\.

<ins>Structure:</ins> A [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) extended with one or more modules that customize its behavior\.

<ins>Type:</ins> Implementation, extension components\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Some aspects are easy to customize | Testability is poor \(too many combinations\) |
| The customized system is relatively light\-weight | Performance is suboptimal |
| Platform\-specific optimizations are supported | Designing good plugin APIs is hard |
| The custom pieces may be written in a different programming language or DSL |  |

<ins>References:</ins> \[[SAP]({{< relref "../appendices/books-referenced.md#sap" >}})\] and \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] [mistakenly]({{< relref "../analytics/ambiguous-patterns.md#microkernel" >}}) call this pattern *Microkernel* and dedicate chapters to it\.

Most systems require some extent of customizability all the way from the basic codec selection by a video player to the screens full of tools and wizards unlocked once you upgrade your subscription plan\. This is achieved by keeping the *core* functionality separate from its extensions, which are developed by either your team or external enthusiasts to modify behavior of the system\. The cost of flexibility is paid in complexity of design – the need to predict which aspects should be customizable and which APIs are good for known \(and unknown\) uses by the extensions\. Heavy communication between the core and *plugins* negatively impacts performance\.

### Performance

Using plugins usually degrades performance\. The effect may be negligible for in\-process plugins \(such as strategies or codecs\) but it becomes noticeable if inter\-process communication or networking is involved\.

The only case for a plugin to improve performance of a system that I can think of is when a part of the client’s business logic moves to a lower layer of a system or to another service forming an [*Ambassador Plugin*]({{< relref "#ambassador-plugin-logic-extension" >}})\. That is similar to the [strategy injection optimization for *Layers*]({{< relref "../basic-metapatterns/layers.md#performance" >}})\. Real\-world examples include:

- The use of *stored procedures* in databases,
- HFT rules and price tables [uploaded](https://www.youtube.com/watch?v=sX2nF1fW7kI) to a network card or FPGA,
- Customization of supplier [*Cells*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}}) for varying needs of their client *Cells* in [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}})\.


<figure>
<a href="/diagrams/Performance/Plugins-injection.png">
<picture>
<source srcset="/diagrams/Performance/Plugins-injection.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Plugins-injection.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Plugins-injection.png" alt="Business logic injection in Layers and Services." loading="lazy" width="1043" height="623" style="width:100%"/>
</picture>
</a>
</figure>

### Dependencies

Each *plugin* depends on the *core*’s *API* \(for *Addons*\) or *SPI* \(for *Plugins*\) for the functionality it extends\. That makes the APIs and SPIs nearly impossible to modify, only to extend, as there tend to be many plugins in the field, some of them out of active development, that rely on any given method of the already published interfaces\.

<figure>
<a href="/diagrams/Dependencies/Plugins.png">
<picture>
<source srcset="/diagrams/Dependencies/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Plugins.png" alt="Each plugin depends on an interface of the core." loading="lazy" width="743" height="324" style="width:100%"/>
</picture>
</a>
</figure>

### Applicability

*Plugins* are <ins>good</ins> for:

- *Product lines\.* The shared *core* functionality and some *plugins* are reused by many flavors of the product\. Per\-client customizations are largely built using existing plugins\.
- *Frameworks\.* A framework is a functional core to be customized by its users\. When shipped with a set of plugins it becomes ready\-to\-use\.
- *Platform\-specific customizations\. Plugins* allow both for native look and feel \(e\.g\. desktop vs mobile vs console\) and for the use of platform\-specific hardware\.


*Plugins* <ins>do not perform well</ins> in:

- *Highly optimized applications\.* Any generic code tends to be inefficient\. A generic API that serves a family of plugins is unlikely to be optimized for the use by any one of them\.


### Relations

<figure>
<a href="/diagrams/Relations/Plugins.png">
<picture>
<source srcset="/diagrams/Relations/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Plugins.png" alt="A monolith with plugins; layers with plugins; a Cell with a plugin." loading="lazy" width="1063" height="603" style="width:100%"/>
</picture>
</a>
</figure>

*Plugins*:

- Implement [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}), or [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\.
- Extend [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}), or [*Cell*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}}) with one or two layers of services\.


## Variants

*Plugins* are omnipresent if we take [*Strategy*](https://refactoring.guru/design-patterns/strategy) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] for a kind of plugin\. They vary:

### By the direction of communication

A plugin may:

- Provide input to the core \(as UI screens and [CLI](https://en.wikipedia.org/wiki/Command-line_interface) connections\)\.
- Receive output from the core \(e\.g\. collection of statistics\)\.
- Participate in both input and output \(like health check instrumentation\)\.
- Take the role of [*Controller*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \(*Orchestrator*\) – the plugin processes events from the core and decides on the core’s further behavior\.
- Be a data processor – the plugin implements a part of a [data processing]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}}) algorithm which is run by the core\. This enables platform\-specific optimizations, like SIMD or DSP\.


### By linkage

Plugins may be *built in* or selected dynamically:

- Every *flavor* \(e\.g\. free, lite, pro, premium\) of a product line incorporates a single set of plugins \(configuration\) built into the application\. Web services often use this kind of customization for [*Multitenancy*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" >}})\.
- Other systems choose and initiate their plugins on startup according to their configuration files or licenses\.
- Still others support attaching and detaching plugins dynamically at runtime\.


### By granularity

Plugins come in different sizes:

- Small functions or classes are built into the core\. They seem to implement the [*Strategy*](https://refactoring.guru/design-patterns/strategy) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] / *Plugins* \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] design patterns\.
- [*Aspects*](https://en.wikipedia.org/wiki/Aspect_(computer_programming)), such as logging and memory management, pervade a system and are accessed from many places in its code\. *Reflection* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] probably belongs there\.
- Modules are plugged in as separate system components\. This kind of *Plugins* matches the topic of this book and is further developed by [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) and [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\.


### By the number of instances

A plugin may be:

- *Mandatory* \(1 instance\), like a piece of algorithm used by the core for a calculation\.
- *Optional* \(0 or 1 instance\), like a smart coloring scheme for a text editor\.
- *Subscriptional* \(0 or more instances\), like log output which may go to a console, the system log, a log file or socket in any combination, or to all of them at once\.


### By execution mode

Plugins may be:

- Linked as a binary code called from within the core\.
- Written in a [*domain\-specific language*](https://en.wikipedia.org/wiki/Domain-specific_language) \(*DSL*\) and [interpreted]({{< relref "../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) by the core\.
- Remote, available as a stand\-alone web service or even hardware component which the core needs to access through a dedicated protocol\.


## Examples

*Plugins* can take different roles and places in the system\. Though their classification and naming has never been well established, we can discern the following kinds of *plugins*, which are non\-exclusive, meaning that a single application may support some or all of them at once:

- A [true *Plugin*]({{< relref "#true-plugin-or-plug-in" >}}) provides custom low\-level functionality, such as a video codec or national tax calculation rule\.
- An [*Ambassador Plugin*]({{< relref "#ambassador-plugin-logic-extension" >}}) injects a part of a given service’s business logic into another service\.
- An [*Extension*]({{< relref "#extension" >}}) modifies its target’s workflow \(use cases\)\.
- An [*Addin*]({{< relref "#addin-or-add-in" >}}) is a deeply integrated optional part of a system’s core logic\.
- An [*Addon*]({{< relref "#inexact-addon-or-add-on" >}}) is an addition that wraps an application to improve user experience\.


### True Plugin \(or Plug\-in\)

<figure>
<a href="/diagrams/Variants/4/Plugins.png">
<picture>
<source srcset="/diagrams/Variants/4/Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Plugins.png" alt="Several low-level plugins are called by a use case running in a system." loading="lazy" width="883" height="383" style="width:100%"/>
</picture>
</a>
</figure>

A true *Plugin* is registered with and called by the system’s *core* to provide \(as an algorithm\) or extend \(as a custom step\) a specific low\-level functionality\. It is usually made by a third party\.

Customizable software often exposes multiple interfaces for different kinds of *Plugins*, some of which are *optional* \(0 or 1 instance\) or *subscriptional* \(multiple instances\) as explained [earlier]({{< relref "#by-the-number-of-instances" >}})\.

Examples: codecs in a video player, country\-specific tax calculation rules in accounting software, or filters in a traffic sniffer\.

### [Ambassador]({{< relref "../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) Plugin, Logic Extension

<figure>
<a href="/diagrams/Variants/4/Ambassador%20Plugin.png">
<picture>
<source srcset="/diagrams/Variants/4/Ambassador%20Plugin.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Ambassador%20Plugin.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Ambassador%20Plugin.png" alt="An ambassador plugin is a part of one service hosted inside another service. When called, it may consult its origin service or make independent decisions." loading="lazy" width="1003" height="283" style="width:100%"/>
</picture>
</a>
</figure>

A service may accept *Plugins* that [act on behalf of peer services](https://www.uber.com/en-UA/blog/microservice-architecture/) \(are [*Ambassadors*]({{< relref "../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}})\) and are implemented by their teams\. That [inverts dependencies]({{< relref "../analytics/comparison-of-architectural-patterns/dependency-inversion-in-architectural-patterns.md" >}}): whoever wants to affect the behavior of your service uses your SPI to inject their code into your service which apart from that remains self\-contained\. As a result, whatever used to be a system\-wide workflow becomes limited to a single subdomain\.

Though an *Ambassador Plugin* \(aka Uber’s [*Logic Extension*](https://www.uber.com/en-UA/blog/microservice-architecture/)\) may call the service on whose behalf it acts, it is preferable performance\-wise for it to make independent decisions without cross\-service calls \(it works like a smart [*Cache*]({{< relref "../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) that provides decisions instead of data\)\. For data\-driven decisions the *Ambassador* may need to opaquely receive and store data \([*Data Extension*](https://www.uber.com/en-UA/blog/microservice-architecture/)\) in its host’s database\.

*Ambassador Plugins* are widely used in Uber’s [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}}) to decouple its [*Cells*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#cell-cluster-domain" >}})\.

### Extension

<figure>
<a href="/diagrams/Variants/4/Extension.png">
<picture>
<source srcset="/diagrams/Variants/4/Extension.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Extension.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Extension.png" alt="An extension is called as a high-level part of a system's use case." loading="lazy" width="843" height="363" style="width:100%"/>
</picture>
</a>
</figure>

An *Extension* changes use cases by modifying the application’s business logic, but it is still called by the application’s *core*\. An *Extension* may install a group of low\-level *Plugins* which add to the system’s low\-level functionality whatever tools the *Extension* relies on\.

Examples: IDE customization\.

### Addin \(or Add\-in\)

<figure>
<a href="/diagrams/Variants/4/Addin.png">
<picture>
<source srcset="/diagrams/Variants/4/Addin.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Addin.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Addin.png" alt="An addin is hosted inside a system and implements a part of its control flow." loading="lazy" width="763" height="264" style="width:94%"/>
</picture>
</a>
</figure>

An *Add\-in* is deeply integrated with the *core* – the system has been originally designed to provide it thorough access to its internals\. It comes from the system’s creators or external teams that commit to the system’s *core*\.

Examples: complex extensions for web browsers or static website generators\.

### \(inexact\) Addon \(or Add\-on\)

<figure>
<a href="/diagrams/Variants/4/Addon.png">
<picture>
<source srcset="/diagrams/Variants/4/Addon.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Addon.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Addon.png" alt="An addon is a layer between a system and its client. It translates a single client request into multiple calls to the underlying system." loading="lazy" width="883" height="364" style="width:100%"/>
</picture>
</a>
</figure>

Though any of the variants of *Plugins* described above may sometimes be called an *addon*, there is a kind of system extension which perfectly matches the meaning of the word\.

A true *Addon* is built on top of the system’s API and calls into the system from outside – it is a kind of external [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) or even [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) for the system\.

Examples: applications that provide [user interface]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) for command\-line tools such as git\.

## Summary

*Plugins* allow for customization of a component’s behavior at the cost of increased complexity, poor testability and somewhat reduced performance\.