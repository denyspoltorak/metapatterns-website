+++
weight = 1
layout = "landing"
title = "The pattern language of software architecture"
description = "This is an online version of the Architectural Metapatterns book which explores system topologies and arranges architectural patterns into a pattern language."
images = ["/cover.png"]
primary_image = "/cover.png"
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

<a href="{{< relref "introduction/system-topologies.md" >}}">

<picture>

<source srcset="/diagrams/Web/Topologies.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Topologies.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Topologies.png" alt="Diagrams of Monolith, Layers, Plugins, Hierarchy, and Services in a system of coordinates that shows partitioning into layers and subdomains." loading="lazy" width="764" height="744"/>

</picture>

System topologies

</a>

<a class="grid-row" href="{{< relref "foundations-of-software-architecture/_index.md" >}}">

<h2>Foundations of software architecture</h2>

</a>

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

<a class="grid-row" href="{{< relref "basic-metapatterns/_index.md" >}}">

<h2>Basic metapatterns</h2>

</a>

<a href="{{< relref "basic-metapatterns/monolith.md" >}}">

<picture>

<source srcset="/diagrams/Web/Monolith.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Monolith.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Monolith.png" alt="A diagram of a monolithic system that blends application, domain rules, generic code, and data in a single component." loading="lazy" width="363" height="243"/>

</picture>

Monolith

</a>

<a href="{{< relref "basic-metapatterns/shards.md" >}}">

<picture>

<source srcset="/diagrams/Web/Shards.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Shards.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Shards.png" alt="A diagram of three interacting instances of a subsystem." loading="lazy" width="366" height="243"/>

</picture>

Shards

</a>

<a href="{{< relref "basic-metapatterns/layers.md" >}}">

<picture>

<source srcset="/diagrams/Web/Layers.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Layers.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Layers.png" alt="A diagram of a system with three layers: application, domain, and database." loading="lazy" width="363" height="245"/>

</picture>

Layers

</a>

<a href="{{< relref "basic-metapatterns/services.md" >}}">

<picture>

<source srcset="/diagrams/Web/Services.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Services.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Services.png" alt="A diagram of three interacting services." loading="lazy" width="363" height="246"/>

</picture>

Services

</a>

<a href="{{< relref "basic-metapatterns/pipeline.md" >}}">

<picture>

<source srcset="/diagrams/Web/Pipeline.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Pipeline.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Pipeline.png" alt="A diagram of a pipeline made from input, three processing steps, and output." loading="lazy" width="364" height="243"/>

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

<img src="/diagrams/Web/Middleware.png" alt="A diagram of three services that use a shared transport." loading="lazy" width="363" height="304"/>

</picture>

Middleware

</a>

<a href="{{< relref "extension-metapatterns/shared-repository.md" >}}">

<picture>

<source srcset="/diagrams/Web/Shared%20Repository.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Shared%20Repository.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Shared%20Repository.png" alt="A diagram of three services above a shared data layer." loading="lazy" width="363" height="304"/>

</picture>

Shared Repository

</a>

<a href="{{< relref "extension-metapatterns/proxy.md" >}}">

<picture>

<source srcset="/diagrams/Web/Proxy.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Proxy.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Proxy.png" alt="A diagram of a client above a proxy above three services." loading="lazy" width="363" height="303"/>

</picture>

Proxy

</a>

<a href="{{< relref "extension-metapatterns/orchestrator.md" >}}">

<picture>

<source srcset="/diagrams/Web/Orchestrator.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Orchestrator.png" alt="A diagram with an integration layer above three services." loading="lazy" width="363" height="303"/>

</picture>

Orchestrator

</a>

<a href="{{< relref "extension-metapatterns/sandwich.md" >}}">

<picture>

<source srcset="/diagrams/Web/Sandwich.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Sandwich.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Sandwich.png" alt="A diagram with an integration layer above three services above a data layer." loading="lazy" width="363" height="305"/>

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

<img src="/diagrams/Web/Layered%20Services.png" alt="A diagram of three three-layered services." loading="lazy" width="443" height="344"/>

</picture>

Layered Services

</a>

<a href="{{< relref "fragmented-metapatterns/polyglot-persistence.md" >}}">

<picture>

<source srcset="/diagrams/Web/Polyglot%20Persistence.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Polyglot%20Persistence.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Polyglot%20Persistence.png" alt="A diagram of three services that share two databases." loading="lazy" width="443" height="341"/>

</picture>

Polyglot Persistence

</a>

<a href="{{< relref "fragmented-metapatterns/backends-for-frontends--bff-.md" >}}">

<picture>

<source srcset="/diagrams/Web/Backends%20for%20Frontends.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Backends%20for%20Frontends.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Backends%20for%20Frontends.png" alt="A diagram with three layers, from top to bottom: mobile and desktop clients; mobile and desktop backends; three services." loading="lazy" width="443" height="343"/>

</picture>

Backends for Frontends

</a>

<a href="{{< relref "fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}">

<picture>

<source srcset="/diagrams/Web/Service-Oriented%20Architecture.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Service-Oriented%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Service-Oriented%20Architecture.png" alt="A diagram of three layers subdivided into two, three, and four services, respectively." loading="lazy" width="443" height="343"/>

</picture>

Service-Oriented Architecture

</a>

<a href="{{< relref "fragmented-metapatterns/hierarchy.md" >}}">

<picture>

<source srcset="/diagrams/Web/Hierarchy.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Hierarchy.png" alt="A diagram of a hierarchy with three layers. There is one component in the top layer, two components below it, and five components in the lowest layer." loading="lazy" width="443" height="343"/>

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

<img src="/diagrams/Web/Plugins.png" alt="A diagram with three layers: two extensions above a large core with business logic above three plugins." loading="lazy" width="323" height="383"/>

</picture>

Plugins

</a>

<a href="{{< relref "implementation-metapatterns/hexagonal-architecture.md" >}}">

<picture>

<source srcset="/diagrams/Web/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Hexagonal%20Architecture.png" alt="A diagram of Hexagonal Architecture with adapters between its core and input, output, database, and libraries." loading="lazy" width="323" height="383"/>

</picture>

Hexagonal Architecture

</a>

<a href="{{< relref "implementation-metapatterns/microkernel.md" >}}">

<picture>

<source srcset="/diagrams/Web/Microkernel.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Microkernel.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Microkernel.png" alt="A diagram of two applications above a microkernel above three provider services." loading="lazy" width="323" height="383"/>

</picture>

Microkernel

</a>

<a href="{{< relref "implementation-metapatterns/mesh.md" >}}">

<picture>

<source srcset="/diagrams/Web/Mesh.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Mesh.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Mesh.png" alt="A diagram of three applications each connected to a node of a mesh. The nodes are communicating to each other." loading="lazy" width="323" height="383"/>

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

<img src="/diagrams/Web/Life%20cycle.png" alt="A diagram that shows a cycle with the following architectures: Monolith, Layers, Layered Services, a Sandwich Cell interacting with orchestrated layered services, and Layers with two databases." loading="lazy" width="1185" height="1123"/>

</picture>

Architecture and product life cycle

</a>

<a href="{{< relref "analytics/real-world-inspirations-for-architectural-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Real-world.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Real-world.negated.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Real-world.png" alt="A diagram of three services with queues of people and luggage above a transport layer with train stations and trains." loading="lazy" width="483" height="463"/>

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

<hr>

<h2 style="text-align: center;">The map of system topologies</h2>

<nav class="map">

<picture>

<source srcset="/diagrams/Web/Map.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Map.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Map.png" alt="The map of system topologies. The vertical asix is partitioning into layers, the horizontal axis is partitioning into services" loading="lazy" width="2964" height="2084"/>

</picture>

<a href="/basic-metapatterns/layers/" aria-label="Layers" style="position: absolute; top: 2%; left: 13%; width: 6.5%; height: 13%;"></a>

<a href="/fragmented-metapatterns/polyglot-persistence/" aria-label="Layers with Polyglot Persistence" style="position: absolute; top: 2%; left: 24.5%; width: 8%; height: 17%;"></a>

<a href="/fragmented-metapatterns/backends-for-frontends--bff-/" aria-label="Layers with Backends for Frontends" style="position: absolute; top: 2%; left: 32.5%; width: 7%; height: 15%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#ddd-style-hexagonal-architecture-onion-architecture-clean-architecture" aria-label="Onion Architecture" style="position: absolute; top: 5%; left: 40%; width: 8%; height: 19.5%;"></a>

<a href="/implementation-metapatterns/microkernel/" aria-label="Microkernel" style="position: absolute; top: 22%; left: 48%; width: 8%; height: 14%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#ports-and-adapters-hexagonal-architecture" aria-label="Hexagonal Architecture" style="position: absolute; top: 30%; left: 39%; width: 8.5%; height: 17.5%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#pedestal" aria-label="Pedestal" style="position: absolute; top: 36.5%; left: 48.5%; width: 7%; height: 11%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" aria-label="Model-View-Controller" style="position: absolute; top: 50%; left: 29.5%; width: 7%; height: 11.5%;"></a>

<a href="/implementation-metapatterns/plugins/" aria-label="Plugins" style="position: absolute; top: 49.5%; left: 38%; width: 6.5%; height: 12.5%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#cell-cluster-domain" aria-label="Cell" style="position: absolute; top: 48%; left: 49.5%; width: 9%; height: 14%;"></a>

<a href="/implementation-metapatterns/hexagonal-architecture/#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" aria-label="Model-View-Presenter" style="position: absolute; top: 30%; left: 19.5%; width: 7%; height: 14.5%;"></a>

<a href="/basic-metapatterns/services/#scaled-service" aria-label="Scaled service" style="position: absolute; top: 23.5%; left: 13%; width: 6.5%; height: 16.5%;"></a>

<a href="/basic-metapatterns/layers/#three-tier-architecture" aria-label="Three-Tier" style="position: absolute; top: 22.5%; left: 5.5%; width: 6.5%; height: 16%;"></a>

<a href="/extension-metapatterns/orchestrator/#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" aria-label="MapReduce" style="position: absolute; top: 39%; left: 5%; width: 7.5%; height: 12.5%;"></a>

<a href="/basic-metapatterns/shards/#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" aria-label="Managed Shards" style="position: absolute; top: 45.5%; left: 14%; width: 7%; height: 14%;"></a>

<a href="/implementation-metapatterns/mesh/#peer-to-peer-networks" aria-label="Peer-to-Peer Mesh" style="position: absolute; top: 60%; left: 14%; width: 7%; height: 12.5%;"></a>

<a href="/basic-metapatterns/monolith/" aria-label="Monolith with a database" style="position: absolute; top: 57.5%; left: 4.5%; width: 8.5%; height: 13%;"></a>

<a href="/basic-metapatterns/shards/#persistent-slice-sharding-shards-partitions-multitenancy-cells-amazon-definition" aria-label="Shards" style="position: absolute; top: 72%; left: 5.5%; width: 6.5%; height: 11%;"></a>

<a href="/basic-metapatterns/monolith/" aria-label="Monolith" style="position: absolute; top: 83.5%; left: 5.5%; width: 6.5%; height: 9%;"></a>

<a href="/basic-metapatterns/shards/#persistent-copy-replica" aria-label="Replicas" style="position: absolute; top: 82.5%; left: 14.5%; width: 7%; height: 11%;"></a>

<a href="/basic-metapatterns/monolith/" aria-label="Monolith with libraries" style="position: absolute; top: 78%; left: 29%; width: 8%; height: 11%;"></a>

<a href="/fragmented-metapatterns/polyglot-persistence/" aria-label="Monolith with Polyglot Persistence" style="position: absolute; top: 62.5%; left: 25%; width: 8%; height: 14.5%;"></a>

<a href="/fragmented-metapatterns/backends-for-frontends--bff-/" aria-label="Monolith with Backends for Frontends" style="position: absolute; top: 62.5%; left: 34%; width: 6.5%; height: 13%;"></a>

<a href="/basic-metapatterns/services/#synchronous-modules-modular-monolith-modulith" aria-label="Modulith with shared code" style="position: absolute; top: 76%; left: 49.5%; width: 9%; height: 11%;"></a>

<a href="/implementation-metapatterns/mesh/#service-mesh" aria-label="Service Mesh" style="position: absolute; top: 65.5%; left: 68%; width: 7%; height: 15%;"></a>

<a href="/basic-metapatterns/services/" aria-label="Services" style="position: absolute; top: 83.5%; left: 90.5%; width: 6.5%; height: 9%;"></a>

<a href="/basic-metapatterns/pipeline/" aria-label="Pipeline" style="position: absolute; top: 72%; left: 88.5%; width: 10.5%; height: 8.5%;"></a>

<a href="/fragmented-metapatterns/polyglot-persistence/" aria-label="Services with Polyglot Persistence" style="position: absolute; top: 50%; left: 80%; width: 9%; height: 15%;"></a>

<a href="/fragmented-metapatterns/backends-for-frontends--bff-/" aria-label="Services with Backends for Frontends" style="position: absolute; top: 35.5%; left: 81%; width: 7%; height: 13%;"></a>

<a href="/fragmented-metapatterns/layered-services/#choreographed-two-layered-services" aria-label="Two-Layered Services" style="position: absolute; top: 42.5%; left: 90.5%; width: 6.5%; height: 12%;"></a>

<a href="/fragmented-metapatterns/layered-services/#orchestrated-three-layered-services" aria-label="Three-Layered Services" style="position: absolute; top: 19.5%; left: 90.5%; width: 6.5%; height: 14.5%;"></a>

<a href="/fragmented-metapatterns/service-oriented-architecture--soa-/" aria-label="Service-Oriented Architecture" style="position: absolute; top: 3%; left: 83%; width: 11%; height: 15%;"></a>

<a href="/fragmented-metapatterns/hierarchy/#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" aria-label="Top-Down Hierarchy" style="position: absolute; top: 3%; left: 65.5%; width: 9.5%; height: 15%;"></a>

<a href="/extension-metapatterns/sandwich/" aria-label="Sandwich" style="position: absolute; top: 7%; left: 55.5%; width: 6.5%; height: 12.5%;"></a>

<a href="/fragmented-metapatterns/hierarchy/#bottom-up-hierarchy-bus-of-buses-network-of-networks" aria-label="Hierarchical Middleware" style="position: absolute; top: 18.5%; left: 63.5%; width: 9.5%; height: 15.5%;"></a>

<a href="/fragmented-metapatterns/hierarchy/#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" aria-label="Cell-Based Architecture" style="position: absolute; top: 19.5%; left: 74.5%; width: 9%; height: 14.5%;"></a>

<a href="/extension-metapatterns/proxy/#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" aria-label="Services with a Gateway" style="position: absolute; top: 35.5%; left: 67.5%; width: 8.5%; height: 13%;"></a>

<a href="/extension-metapatterns/orchestrator/" aria-label="Orchestrated Services" style="position: absolute; top: 35.5%; left: 58.5%; width: 8.5%; height: 13%;"></a>

<a href="/extension-metapatterns/shared-repository/" aria-label="Services with a Shared Repository" style="position: absolute; top: 50%; left: 58.5%; width: 8.5%; height: 15%;"></a>

<a href="/extension-metapatterns/middleware/" aria-label="Services with a Middleware" style="position: absolute; top: 50%; left: 67.5%; width: 8.5%; height: 13%;"></a>

<a href="/basic-metapatterns/layers/#application-use-cases-or-integration" aria-label="Use cases" style="position: absolute; top: 96.5%; left: 8.5%; width: 15.5%; height: 3.5%;"></a>

<a href="/basic-metapatterns/layers/#domain-business-rules-or-model" aria-label="Domain logic" style="position: absolute; top: 96.5%; left: 30%; width: 15.5%; height: 3.5%;"></a>

<a href="/basic-metapatterns/layers/#data-persistence" aria-label="Data" style="position: absolute; top: 96.5%; left: 73%; width: 10.5%; height: 3.5%;"></a>

</nav>

<hr>

<h2 id="book" style="text-align: center;">The book</h2>

<img src="cover.png" alt="Cover of Architectural Metapatterns" class="cover-img" loading="lazy" width="1240" height="1755" sizes="auto" srcset="cover_6.png 207w, cover_5.png 248w, cover_4.png 310w, cover_3.png 413w, cover.png 1240w">

This website is an online version of my book _Architectural Metapatterns: The Pattern Language of Software Architecture_ which can be downloaded from [GitHub](https://github.com/denyspoltorak/metapatterns) or [Leanpub](https://leanpub.com/metapatterns).

It is a compendium of architectural patterns which sorts them out into a tree-like hierarchy based on the pattern's structure and function. This [allows for grouping](/introduction/metapatterns/) hundreds of patterns into less then 20 classes and exploring the common features, applicability, and performance of each class.

It also includes supplementary topics that range from the [discussion on the nature of complexity](/foundations-of-software-architecture/modules-and-complexity/) to the comparison of [orchestration](/foundations-of-software-architecture/arranging-communication/orchestration/), [choreography](/foundations-of-software-architecture/arranging-communication/choreography/), and [integration through shared data](/foundations-of-software-architecture/arranging-communication/shared-data/). Aside of that, there is a wide range of [evolutions](/appendices/evolutions-of-architectures/) which show how a system may change under different forces.

_Architectural Metapatterns_ is AI-free, 440 pages long, and includes hundreds of [box-and-arrow diagrams](/introduction/about-this-book/#diagrams).

If you like the book or website, please tell your friends about them. _Knowledge must be free!_