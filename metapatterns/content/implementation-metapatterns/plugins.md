+++
weight = 5
title = "Plugins"
description = "Plugins let a client customize select aspects of a system's behavior. Addons build their application logic on top of whatever the modified system provides."
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
<img src="/diagrams/Main/Plugins.png" alt="Plugins" loading="lazy" width="942" height="554" style="width:100%"/>
</picture>
</a>
</figure>

*Overspecialize, and you breed in weakness\.* Customize the system through attachable modules\.

<ins>Known as:</ins> Plug\-In Architecture \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], \(misapplied\) Microkernel \(Architecture\) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}}), [SAP]({{< relref "../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\], Plugin \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\], Addons, Strategy \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\], Reflection \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\], Aspects, Hooks\.

<ins>Variants:</ins> [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}) and [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}}) got dedicated chapters\. Plugins have many variations\.

<ins>Structure:</ins> A [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) extended with one or more modules that customize its behavior\.

<ins>Type:</ins> Implementation, extension\.

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

The only case for a plugin to improve performance of a system that I can think up is when a part of the client’s business logic moves to a plugin of a lower layer of a system\. That is similar to the [strategy injection optimization for *Layers*]({{< relref "../basic-metapatterns/layers.md#performance" >}})\. Real\-world examples include:

- The use of *stored procedures* in databases,
- HFT rules and price tables [uploaded](https://www.youtube.com/watch?v=sX2nF1fW7kI) to a network card or FPGA,
- Customization of supplier services for varying needs of their client services in [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}})\.


<figure>
<a href="/diagrams/Performance/Plugins-injection.png">
<picture>
<source srcset="/diagrams/Performance/Plugins-injection.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Plugins-injection.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Plugins-injection.png" alt="Plugins-injection" loading="lazy" width="1003" height="321" style="width:100%"/>
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
<img src="/diagrams/Dependencies/Plugins.png" alt="Plugins" loading="lazy" width="743" height="324" style="width:100%"/>
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
<img src="/diagrams/Relations/Plugins.png" alt="Plugins" loading="lazy" width="643" height="603" style="width:68%"/>
</picture>
</a>
</figure>

*Plugins*:

- Implement [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or sometimes [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Extend [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) with one or two layers of services\.


## Variants

*Plugins* are highly variable and omnipresent if we take *Strategy* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] for a kind of plugin:

### By the direction of control

The terminology is not settled, but according to what I found over the Web:

- True *Plugins* are registered with and called by the system’s *core*, they may call back into the core or return results – they are parts of the system\.
- *Addons* are built on top of the system’s API and call into the system from outside – they are more like external [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) for the system\.


### By abstractness

A system may use plugins that are more abstract than the core, less abstract or both:

- High\-level plugins tend to be related to user experience, statistics, or metadata\. They use the core in their own business logic\. *Addons* belong here\.
- Low\-level plugins encapsulate algorithms that are used by the core’s business logic\.
- Some customizations require multiple plugins: a high\-level user\-facing addon may rely on algorithms or pieces of business logic implemented by several complementary low\-level plugins\.


### By the direction of communication

A plugin may:

- Provide input to the core \(as UI screens and [CLI](https://en.wikipedia.org/wiki/Command-line_interface) connections\)\.
- Receive output from the core \(e\.g\. collection of statistics\)\.
- Participate in both input and output \(like health check instrumentation\)\.
- Take the role of [*Controller*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \(*Orchestrator*\) – the plugin processes events from the core and decides on the core’s further behavior\.
- Be a data processor – the plugin implements a part of a [data processing]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}}) algorithm which is run by the core\. This enables platform\-specific optimizations, like SIMD or DSP\.


### By linkage

Plugins may be *built in* or selected dynamically:

- Every *flavor* \(e\.g\. free, lite, pro, premium\) of a product line incorporates a single set of plugins \(configuration\) built into the application\.
- Other systems choose and initiate their plugins on startup according to their configuration files or licenses\.
- Still others support attaching and detaching plugins dynamically at runtime\.


### By granularity

Plugins come in different sizes:

- Small functions or classes are built into the core\. They seem to implement the *Strategy* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] / *Plugins* \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] design patterns\.
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
- Communicated with over a network\.


## Summary

*Plugins* allow for customization of a component’s behavior at the cost of increased complexity, poor testability and somewhat reduced performance\.