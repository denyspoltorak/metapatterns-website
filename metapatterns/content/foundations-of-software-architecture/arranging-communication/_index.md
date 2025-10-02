+++
weight = 4
title = "Arranging communication"
description = "Components can be integrated through orchestration, choreography, or shared data. These approaches emerge at every level, from code to system design."
images = ["/diagrams/Communication/Monolith%20to%20Services.svg"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Arranging communication

As a project grows, it tends to become subdivided into services, modules, or whatever you call the components that match its subdomains \(or *bounded contexts*, if you prefer the \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] convention\)\. Still, there remain system\-wide use cases that require collaboration from many or all of the system’s parts – otherwise the components don’t make a single system\. Let’s see how they can be integrated\.

<figure>
<a href="/diagrams/Communication/Monolith%20to%20Services.png">
<picture>
<source srcset="/diagrams/Communication/Monolith%20to%20Services.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Communication/Monolith%20to%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Monolith%20to%20Services.png" alt="Monolith to Services" loading="lazy" width="1062" height="227" style="width:100%"/>
</picture>
</a>
</figure>

As integration is not unique to distributed systems – it is present even in smaller programs that need to make data, functions, and classes work together – we’ll take a look at programming and architectural paradigms next\.

## Contents:

<nav>

- [Programming and architectural paradigms]({{< relref "foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}})
- [Orchestration]({{< relref "foundations-of-software-architecture/arranging-communication/orchestration.md" >}})
- [Choreography]({{< relref "foundations-of-software-architecture/arranging-communication/choreography.md" >}})
- [Shared data]({{< relref "foundations-of-software-architecture/arranging-communication/shared-data.md" >}})
- [Comparison of communication styles]({{< relref "foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}})

</nav>

<nav>

| \<\< [Four kinds of software]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md" >}}) | ^ [Foundations of software architecture]({{< relref "../../foundations-of-software-architecture/_index.md" >}}) ^ | [Programming and architectural paradigms]({{< relref "../../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}}) \>\> |
| --- | --- | --- |

</nav>