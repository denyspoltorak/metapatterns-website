+++
weight = 4
title = "Arranging communication"
description = "Components can be integrated through orchestration, choreography, or shared data. These approaches emerge at every level, from code to system design."
images = ["/diagrams/Communication/Monolith%20to%20Services.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Arranging communication {anchor=false}

As a project grows, it tends to become subdivided into services, modules, or whatever you call the components that match its subdomains \(or *bounded contexts*, if you prefer the \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] convention\)\. Still, there remain system\-wide use cases that require collaboration from many or all of the system’s parts – otherwise the components don’t make a single system\. Let’s see how they can be integrated\.

<figure>
<a href="/diagrams/Communication/Monolith%20to%20Services.png">
<picture>
<source srcset="/diagrams/Communication/Monolith%20to%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Monolith%20to%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Monolith%20to%20Services.png" alt="Monolith to Services" loading="lazy" width="1062" height="227" style="width:100%"/>
</picture>
</a>
</figure>

As integration is not unique to distributed systems – it is present even in smaller programs that need to make data, functions, and classes work together – we’ll take a look at programming and architectural paradigms next\.

## Contents:

<nav class="grid3">

<a class="grid-row" href="{{< relref "foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}}">

Programming and architectural paradigms

</a>

<a href="{{< relref "foundations-of-software-architecture/arranging-communication/orchestration.md" >}}">

Orchestration

</a>

<a href="{{< relref "foundations-of-software-architecture/arranging-communication/choreography.md" >}}">

Choreography

</a>

<a href="{{< relref "foundations-of-software-architecture/arranging-communication/shared-data.md" >}}">

Shared data

</a>

<a class="grid-row" href="{{< relref "foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}}">

Comparison of communication styles

</a>

</nav>