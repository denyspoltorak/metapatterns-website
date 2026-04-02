+++
weight = 1
layout = "landing"
title = "The pattern language of software architecture"
description = "This is an online version of the Architectural Metapatterns book which explores system topologies and arranges architectural patterns into a pattern language."
images = ["/diagrams/Web/og/Favicon-plain.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.5
+++

# The pattern language of software architecture {anchor=false}

Patterns of software architecture are all interrelated (*no pattern is an island*). You can rarely make a product in a pure architectural style, and the chances for it to survive undistorted over years are negligible. Software grows iteratively and adapts to its environment.

[*Architectural Metapatterns*](https://leanpub.com/metapatterns) is all about patterns and their relations. It generalizes hundreds of individual patterns into several wider classes ([*metapatterns*]({{< relref "introduction/metapatterns.md" >}})) each of which can be applied to a local or distributed system to change its properties in a certain way. *Rinse and repeat.*

The content is lavishly illustrated with intuitive NoUML diagrams. It’s concise and AI-free.

*Have a good time!*

<nav class="grid5">

<a class="grid-row" href="https://speakerdeck.com/denyspoltorak">

<h2>Presentations (outdated)</h2>

</a>

<a href="https://speakerdeck.com/denyspoltorak/patterns-of-patterns-c322149c-fcdd-4df6-8c6c-f273c7ab17dd">

<img src="/diagrams/Web/slides/Patterns.png" alt="Patterns of Patterns" width="480" height="270"/>

</a>

<a href="https://speakerdeck.com/denyspoltorak/basic-architectures">

<img src="/diagrams/Web/slides/Basic.png" alt="Basic Architectures" width="480" height="270"/>

</a>

<a href="https://speakerdeck.com/denyspoltorak/architectural-extensions-c3e506c0-6472-4e2c-8113-b3d8ffa20d63">

<img src="/diagrams/Web/slides/Extensions.png" alt="Architectural Extensions" width="480" height="270"/>

</a>

<a href="https://speakerdeck.com/denyspoltorak/fragmented-architectures-bf4d129f-4cd1-46bf-9fb0-248879872b25">

<img src="/diagrams/Web/slides/Fragmented.png" alt="Fragmented Architectures" width="480" height="270"/>

</a>

<a href="https://speakerdeck.com/denyspoltorak/implementation-patterns">

<img src="/diagrams/Web/slides/Implementation.png" alt="Implementation Patterns" width="480" height="270"/>

</a>

</nav>

<hr>

<nav class="grid5">

<a class="grid-row" href="{{< relref "introduction/_index.md" >}}">

<h2>Introduction</h2>

</a>

<a href="{{< relref "introduction/about-this-book.md" >}}">

<picture>

<source srcset="/diagrams/Web/About.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/About.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/About.png" alt="A text: there are too many patterns!" loading="lazy" width="720" height="680"/>

</picture>

About this book

</a>

<a href="{{< relref "introduction/metapatterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Metapatterns.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Metapatterns.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Metapatterns.png" alt="Abstraction, subdomain, and sharding axes with the following text between them: your pattern here." loading="lazy" width="375" height="363"/>

</picture>

Metapatterns

</a>

<a class="grid-row" href="{{< relref "foundations-of-software-architecture/_index.md" >}}">

<h2>Foundations of software architecture</h2>

</a>

<a href="{{< relref "foundations-of-software-architecture/modules-and-complexity.md" >}}">

<picture>

<source srcset="/diagrams/Web/Complexity.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Complexity.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Complexity.png" alt="Three components each encapsulating a graph of nodes." loading="lazy" width="543" height="564"/>

</picture>

Modules and complexity

</a>

<a href="{{< relref "foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}">

<picture>

<source srcset="/diagrams/Web/Forces.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Forces.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Forces.png" alt="Messaging in a three-layered system with the lower layer making multiple calls to hardware." loading="lazy" width="409" height="403"/>

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

<img src="/diagrams/Web/Communication.png" alt="C client above three services with question marks between the components." loading="lazy" width="263" height="263"/>

</picture>

Arranging communication

</a>

<a class="grid-row" href="{{< relref "basic-metapatterns/_index.md" >}}">

<h2>Basic metapatterns</h2>

</a>

<a href="{{< relref "basic-metapatterns/monolith.md" >}}">

<picture>

<source srcset="/diagrams/Web/Monolith.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Monolith.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Monolith.png" alt="A monolithic system that blends application, domain rules, generic code, and data in a single component." loading="lazy" width="363" height="243"/>

</picture>

Monolith

</a>

<a href="{{< relref "basic-metapatterns/shards.md" >}}">

<picture>

<source srcset="/diagrams/Web/Shards.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Shards.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Shards.png" alt="Three interacting instances of a subsystem." loading="lazy" width="366" height="243"/>

</picture>

Shards

</a>

<a href="{{< relref "basic-metapatterns/layers.md" >}}">

<picture>

<source srcset="/diagrams/Web/Layers.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Layers.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Layers.png" alt="A system with three layers: application, domain, and database." loading="lazy" width="363" height="245"/>

</picture>

Layers

</a>

<a href="{{< relref "basic-metapatterns/services.md" >}}">

<picture>

<source srcset="/diagrams/Web/Services.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Services.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Services.png" alt="Three interacting services." loading="lazy" width="363" height="246"/>

</picture>

Services

</a>

<a href="{{< relref "basic-metapatterns/pipeline.md" >}}">

<picture>

<source srcset="/diagrams/Web/Pipeline.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Pipeline.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Pipeline.png" alt="A pipeline comprising input, three processing steps, and output." loading="lazy" width="364" height="243"/>

</picture>

Pipeline

</a>

<a class="grid-row" href="{{< relref "extension-metapatterns/_index.md" >}}">

<h2>Extension metapatterns</h2>

</a>

<a href="{{< relref "extension-metapatterns/middleware.md" >}}">

<picture>

<source srcset="/diagrams/Web/Middleware.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Middleware.png" alt="Three services that use a shared transport." loading="lazy" width="363" height="304"/>

</picture>

Middleware

</a>

<a href="{{< relref "extension-metapatterns/shared-repository.md" >}}">

<picture>

<source srcset="/diagrams/Web/Shared%20Repository.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Shared%20Repository.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Shared%20Repository.png" alt="Three services above a shared data layer." loading="lazy" width="363" height="304"/>

</picture>

Shared Repository

</a>

<a href="{{< relref "extension-metapatterns/proxy.md" >}}">

<picture>

<source srcset="/diagrams/Web/Proxy.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Proxy.png" alt="A client above a proxy above three services." loading="lazy" width="363" height="303"/>

</picture>

Proxy

</a>

<a href="{{< relref "extension-metapatterns/orchestrator.md" >}}">

<picture>

<source srcset="/diagrams/Web/Orchestrator.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Orchestrator.png" alt="An integration layer above three services." loading="lazy" width="363" height="303"/>

</picture>

Orchestrator

</a>

<a href="{{< relref "extension-metapatterns/sandwich.md" >}}">

<picture>

<source srcset="/diagrams/Web/Sandwich.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Sandwich.png" alt="An integration layer above three services above a data layer." loading="lazy" width="363" height="305"/>

</picture>

Sandwich

</a>

<a class="grid-row" href="{{< relref "fragmented-metapatterns/_index.md" >}}">

<h2>Fragmented metapatterns</h2>

</a>

<a href="{{< relref "fragmented-metapatterns/layered-services.md" >}}">

<picture>

<source srcset="/diagrams/Web/Layered%20Services.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Layered%20Services.png" alt="Three three-layered services." loading="lazy" width="443" height="344"/>

</picture>

Layered Services

</a>

<a href="{{< relref "fragmented-metapatterns/polyglot-persistence.md" >}}">

<picture>

<source srcset="/diagrams/Web/Polyglot%20Persistence.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Polyglot%20Persistence.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Polyglot%20Persistence.png" alt="Three services which share two databases." loading="lazy" width="443" height="341"/>

</picture>

Polyglot Persistence

</a>

<a href="{{< relref "fragmented-metapatterns/backends-for-frontends--bff-.md" >}}">

<picture>

<source srcset="/diagrams/Web/Backends%20for%20Frontends.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Backends%20for%20Frontends.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Backends%20for%20Frontends.png" alt="Three layers, from top to bottom: mobile and desktop clients; mobile and desktop backends; three services." loading="lazy" width="443" height="343"/>

</picture>

Backends for Frontends

</a>

<a href="{{< relref "fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}">

<picture>

<source srcset="/diagrams/Web/Service-Oriented%20Architecture.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Service-Oriented%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Service-Oriented%20Architecture.png" alt="Three layers subdivided into two, three, and four services, respectively." loading="lazy" width="443" height="343"/>

</picture>

Service-Oriented Architecture

</a>

<a href="{{< relref "fragmented-metapatterns/hierarchy.md" >}}">

<picture>

<source srcset="/diagrams/Web/Hierarchy.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Hierarchy.png" alt="A hierarchy with three layers. There is one component in the top layer, two components below it, and five components in the lowest layer." loading="lazy" width="443" height="343"/>

</picture>

Hierarchy

</a>

<a class="grid-row" href="{{< relref "implementation-metapatterns/_index.md" >}}">

<h2>Implementation metapatterns</h2>

</a>

<a href="{{< relref "implementation-metapatterns/plugins.md" >}}">

<picture>

<source srcset="/diagrams/Web/Plugins.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Plugins.png" alt="Three layers: two extensions above a large core with business logic above three plugins." loading="lazy" width="323" height="383"/>

</picture>

Plugins

</a>

<a href="{{< relref "implementation-metapatterns/hexagonal-architecture.md" >}}">

<picture>

<source srcset="/diagrams/Web/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Hexagonal%20Architecture.png" alt="Hexagonal Architecture with adapters between its core and input, output, database, and libraries." loading="lazy" width="323" height="383"/>

</picture>

Hexagonal Architecture

</a>

<a href="{{< relref "implementation-metapatterns/microkernel.md" >}}">

<picture>

<source srcset="/diagrams/Web/Microkernel.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Microkernel.png" alt="Two applications above a microkernel above three provider services." loading="lazy" width="323" height="383"/>

</picture>

Microkernel

</a>

<a href="{{< relref "implementation-metapatterns/mesh.md" >}}">

<picture>

<source srcset="/diagrams/Web/Mesh.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Mesh.png" alt="Three applications each connected to a node of a mesh. The nodes are communicating to each other." loading="lazy" width="323" height="383"/>

</picture>

Mesh

</a>

<a class="grid-row" href="{{< relref "analytics/_index.md" >}}">

<h2>Analytics</h2>

</a>

<a href="{{< relref "analytics/comparison-of-architectural-patterns/_index.md" >}}">

<picture>

<source srcset="/diagrams/Web/Comparison.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Comparison.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Comparison.png" alt="Diagrams of services sharing a dataset, a pipeline, dependency inversion in an operating system with device drivers, and an adapter between a client and a service." loading="lazy" width="665" height="645"/>

</picture>

Comparison of architectural patterns

</a>

<a href="{{< relref "analytics/ambiguous-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Ambiguous.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Ambiguous.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Ambiguous.png" alt="Five diagrams of various systems called monoliths." loading="lazy" width="1003" height="983"/>

</picture>

Ambiguous patterns

</a>

<a href="{{< relref "analytics/architecture-and-product-life-cycle.md" >}}">

<picture>

<source srcset="/diagrams/Web/Life%20cycle.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Life%20cycle.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Life%20cycle.png" alt="A cycle with the following architectures: Monolith, Layers, Layered Services, a Sandwich Cell interacting with orchestrated layered services, and Layers with two databases." loading="lazy" width="1185" height="1123"/>

</picture>

Architecture and product life cycle

</a>

<a href="{{< relref "analytics/real-world-inspirations-for-architectural-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Real-world.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Real-world.negated.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Real-world.png" alt="Three services with queues of people and luggage above a transport layer with train stations and trains." loading="lazy" width="483" height="463"/>

</picture>

Real-world inspirations for architectural patterns

</a>

<a href="{{< relref "analytics/the-heart-of-software-architecture/_index.md" >}}">

<picture>

<source srcset="/diagrams/Web/Heart.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Heart.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Heart.png" alt="A plot of pain level of maintaining a project against the project's size, with different architectures being optimal for different project sizes." loading="lazy" width="403" height="384"/>

</picture>

The heart of software architecture

</a>

<a class="grid-row" href="{{< relref "appendices/_index.md" >}}">

<h2>Appendices</h2>

</a>

<a href="{{< relref "appendices/acknowledgements.md" >}}">

Acknowledgements

</a>

<a href="{{< relref "appendices/books-referenced.md" >}}">

Books referenced

</a>

<a href="{{< relref "appendices/copyright.md" >}}">

Copyright

</a>

<a href="{{< relref "appendices/disclaimer.md" >}}">

Disclaimer

</a>

<a href="{{< relref "appendices/evolutions-of-architectures/_index.md" >}}">

Evolutions of architectures

</a>

<a href="{{< relref "appendices/format-of-a-metapattern.md" >}}">

Format of a metapattern

</a>

<a href="{{< relref "appendices/glossary.md" >}}">

Glossary

</a>

<a href="{{< relref "appendices/history-of-changes.md" >}}">

History of changes

</a>

<a href="{{< relref "appendices/index-of-patterns.md" >}}">

Index of patterns

</a>

</nav>