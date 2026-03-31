+++
weight = 2
title = "Foundations of software architecture"
description = "This part of the book discusses fundamental topics: complexity, non-functional requirements (forces), communication paradigms, and types of software systems."
images = ["/diagrams/Web/og/Favicon-plain.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Foundations of software architecture {anchor=false}

This part defines some ideas which are used throughout the book:

- [Complexity]({{< relref "../foundations-of-software-architecture/modules-and-complexity.md" >}}) and its relation to modules, coupling and cohesion\.
- [Forces]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}) \(non\-functional requirements\), their conflicts, and how those are resolved through asynchronous communication and distribution of system components\.
- Different [kinds of software systems]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md" >}}): [control]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}), [interactive]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#interactive-soft-real-time-user-input" >}}), [streaming]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}), and [computational]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#computational-single-run-user-input" >}})\.
- [Communication paradigms]({{< relref "../foundations-of-software-architecture/arranging-communication/_index.md" >}}): [orchestration]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}), [choreography]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) and [shared data]({{< relref "../foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\.


Please feel free to skip \(through\) it as you probably know most of them quite well\.

## Contents:

<nav class="grid3">

<a href="{{< relref "foundations-of-software-architecture/modules-and-complexity.md" >}}">

<picture>

<source srcset="/diagrams/Web/Complexity.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Complexity.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Complexity.png" alt="A diagram of three components each encapsulating a graph of nodes." loading="lazy" width="543" height="564"/>

</picture>

Modules and complexity

</a>

<a href="{{< relref "foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}">

<picture>

<source srcset="/diagrams/Web/Forces.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Forces.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Forces.png" alt="A diagram of messaging in a three-layered system with the lower layer making multiple calls to hardware." loading="lazy" width="409" height="403"/>

</picture>

Forces, asynchronicity, and distribution

</a>

<a href="{{< relref "foundations-of-software-architecture/four-kinds-of-software.md" >}}">

<picture>

<source srcset="/diagrams/Web/4Kinds.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/4Kinds.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/4Kinds.png" alt="Diagrams of control, interactive, streaming, and computational systems." loading="lazy" width="622" height="627"/>

</picture>

Four kinds of software

</a>

<a href="{{< relref "foundations-of-software-architecture/arranging-communication/_index.md" >}}">

<picture>

<source srcset="/diagrams/Web/Communication.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Communication.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Communication.png" alt="A diagram of a client above three services with question marks between the components." loading="lazy" width="263" height="263"/>

</picture>

Arranging communication

</a>

</nav>