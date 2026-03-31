+++
weight = 1
title = "Comparison of architectural patterns"
description = "This chapter explores the ways patterns share functionality or data between their components, build pipelines, and use dependency inversion or indirection."
images = ["/diagrams/Web/og/Comparison.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Comparison of architectural patterns {anchor=false}

This chapter is a compilation of small sections each of which examines one aspect of the [architectural patterns included in this book]({{< relref "../../appendices/index-of-patterns.md" >}})\. It shows the value of having a list of metapatterns to iterate over and analyze\.

## Contents:

<nav class="grid3">

<a href="{{< relref "analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}">

<picture>

<source srcset="/diagrams/Web/Sharing.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Sharing.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Sharing.png" alt="A diagram of two services with private databases and one database shared by the services." loading="lazy" width="263" height="263"/>

</picture>

Sharing functionality or data among services

</a>

<a href="{{< relref "analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Pipelineliness.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Pipelineliness.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Pipelineliness.png" alt="A diagram of a client passing data to a service, which passes it to its database, which passes it to another service's database, from which it goes to the other service, and back to the client." loading="lazy" width="263" height="263"/>

</picture>

Pipelines in architectural patterns

</a>

<a href="{{< relref "analytics/comparison-of-architectural-patterns/dependency-inversion-in-architectural-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/DI.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/DI.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/DI.png" alt="A diagram of polymorphic device drivers in an operating system." loading="lazy" width="262" height="263"/>

</picture>

Dependency inversion in architectural patterns

</a>

<a href="{{< relref "analytics/comparison-of-architectural-patterns/indirection-in-commands-and-queries.md" >}}">

<picture>

<source srcset="/diagrams/Web/Indirection.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Indirection.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Indirection.png" alt="A diagram of a client accessing a service through an adapter." loading="lazy" width="263" height="263"/>

</picture>

Indirection in commands and queries

</a>

</nav>