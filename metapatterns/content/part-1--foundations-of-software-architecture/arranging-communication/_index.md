+++
weight = 4
title = "Arranging communication"
description = "Components can be integrated through orchestration, choreography or shared data. These approaches emerge at every level: from code to system design."
bookCollapseSection = true
+++

# Arranging communication

As a project grows, it tends to become subdivided into services, modules, or whatever you call the components that match its subdomains \(or *bounded contexts*, if you prefer the \[[DDD]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] convention\)\. Still, there remain system\-wide use cases that require collaboration from many or all of the system’s parts – otherwise the components don’t make a single system\. Let’s see how they can be integrated\.

<figure style="text-align:center">
<a href="/Communication/Monolith%20to%20Services.png" style="outline:none">
<img src="/Communication/Monolith%20to%20Services.png" alt="Monolith to Services" style="width:100%"/>
</a>
</figure>

As integration is not unique to distributed systems – it is present even in smaller programs that need to make data, functions, and classes work together – we’ll take a look at programming and architectural paradigms next\.

## Contents:

<nav>

- [Programming and architectural paradigms]({{< relref "part-1--foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}})
- [Orchestration]({{< relref "part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}})
- [Choreography]({{< relref "part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}})
- [Shared data]({{< relref "part-1--foundations-of-software-architecture/arranging-communication/shared-data.md" >}})
- [Comparison of communication styles]({{< relref "part-1--foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}})

</nav>



<nav>

| \<\< [Four kinds of software]({{< relref "../../part-1--foundations-of-software-architecture/four-kinds-of-software.md" >}}) | ^ [Part 1\. Foundations of software architecture]({{< relref "../../part-1--foundations-of-software-architecture/_index.md" >}}) ^ | [Programming and architectural paradigms]({{< relref "../../part-1--foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}}) \>\> |
| --- | --- | --- |

</nav>



