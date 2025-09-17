+++
weight = 1
title = "The pattern language of software architecture"
description = "The Metapatterns website is a compendium and classification of architectural patterns based on the assumption that structure determines function."
bookCollapseSection = true
[sitemap]
  priority = 0.5
+++

# The pattern language of software architecture

Patterns of software architecture are interrelated (*no pattern is an island*). You can rarely make a product in a pure architectural style, and the chances for it to survive undistorted over years are negligible. Software grows iteratively and adapts to its environment.

*Architectural Metapatterns* is all about patterns and their relations. It generalizes hundreds of individual patterns into several wider classes (*metapatterns*) each of which can be applied to a local or distributed system to change its properties in a certain way. *Rinse and repeat.*

The content is lavishly illustrated with intuitive NoSQL diagrams. It’s terse and AI-free.

*Have a good time!*


<nav>


### [Introduction]({{< relref "introduction/_index.md" >}})

- [About this book]({{< relref "introduction/about-this-book.md" >}})
- [Metapatterns]({{< relref "introduction/metapatterns.md" >}})

### [Foundations of software architecture]({{< relref "foundations-of-software-architecture/_index.md" >}})

- [Modules and complexity]({{< relref "foundations-of-software-architecture/modules-and-complexity.md" >}})
- [Forces, asynchronicity, and distribution]({{< relref "foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}})
- [Four kinds of software]({{< relref "foundations-of-software-architecture/four-kinds-of-software.md" >}})
- [Arranging communication]({{< relref "foundations-of-software-architecture/arranging-communication/_index.md" >}})
  - [Programming and architectural paradigms]({{< relref "foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}})
  - [Orchestration]({{< relref "foundations-of-software-architecture/arranging-communication/orchestration.md" >}})
  - [Choreography]({{< relref "foundations-of-software-architecture/arranging-communication/choreography.md" >}})
  - [Shared data]({{< relref "foundations-of-software-architecture/arranging-communication/shared-data.md" >}})
  - [Comparison of communication styles]({{< relref "foundations-of-software-architecture/arranging-communication/comparison-of-communication-styles.md" >}})

### [Basic metapatterns]({{< relref "basic-metapatterns/_index.md" >}})

- [Monolith]({{< relref "basic-metapatterns/monolith.md" >}})
- [Shards]({{< relref "basic-metapatterns/shards.md" >}})
- [Layers]({{< relref "basic-metapatterns/layers.md" >}})
- [Services]({{< relref "basic-metapatterns/services.md" >}})
- [Pipeline]({{< relref "basic-metapatterns/pipeline.md" >}})

### [Extension metapatterns]({{< relref "extension-metapatterns/_index.md" >}})

- [Middleware]({{< relref "extension-metapatterns/middleware.md" >}})
- [Shared Repository]({{< relref "extension-metapatterns/shared-repository.md" >}})
- [Proxy]({{< relref "extension-metapatterns/proxy.md" >}})
- [Orchestrator]({{< relref "extension-metapatterns/orchestrator.md" >}})
- [Combined Component]({{< relref "extension-metapatterns/combined-component.md" >}})

### [Fragmented metapatterns]({{< relref "fragmented-metapatterns/_index.md" >}})

- [Layered Services]({{< relref "fragmented-metapatterns/layered-services.md" >}})
- [Polyglot Persistence]({{< relref "fragmented-metapatterns/polyglot-persistence.md" >}})
- [Backends for Frontends (BFF)]({{< relref "fragmented-metapatterns/backends-for-frontends--bff-.md" >}})
- [Service-Oriented Architecture (SOA)]({{< relref "fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})
- [Hierarchy]({{< relref "fragmented-metapatterns/hierarchy.md" >}})

### [Implementation metapatterns]({{< relref "implementation-metapatterns/_index.md" >}})

- [Plugins]({{< relref "implementation-metapatterns/plugins.md" >}})
- [Hexagonal Architecture]({{< relref "implementation-metapatterns/hexagonal-architecture.md" >}})
- [Microkernel]({{< relref "implementation-metapatterns/microkernel.md" >}})
- [Mesh]({{< relref "implementation-metapatterns/mesh.md" >}})

### [Analytics]({{< relref "analytics/_index.md" >}})

- [Comparison of architectural patterns]({{< relref "analytics/comparison-of-architectural-patterns/_index.md" >}})
  - [Sharing functionality or data among services]({{< relref "analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}})
  - [Pipelines in architectural patterns]({{< relref "analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}})
  - [Dependency inversion in architectural patterns]({{< relref "analytics/comparison-of-architectural-patterns/dependency-inversion-in-architectural-patterns.md" >}})
  - [Indirection in commands and queries]({{< relref "analytics/comparison-of-architectural-patterns/indirection-in-commands-and-queries.md" >}})
- [Ambiguous patterns]({{< relref "analytics/ambiguous-patterns.md" >}})
- [Architecture and product life cycle]({{< relref "analytics/architecture-and-product-life-cycle.md" >}})
- [Real-world inspirations for architectural patterns]({{< relref "analytics/real-world-inspirations-for-architectural-patterns.md" >}})
- [The heart of software architecture]({{< relref "analytics/the-heart-of-software-architecture/_index.md" >}})
  - [Cohesers and decouplers]({{< relref "analytics/the-heart-of-software-architecture/cohesers-and-decouplers.md" >}})
  - [Deconstructing patterns]({{< relref "analytics/the-heart-of-software-architecture/deconstructing-patterns.md" >}})
  - [Choose your own architecture]({{< relref "analytics/the-heart-of-software-architecture/choose-your-own-architecture.md" >}})

### [Appendices]({{< relref "appendices/_index.md" >}})

- [Acknowledgements]({{< relref "appendices/acknowledgements.md" >}})
- [Books referenced]({{< relref "appendices/books-referenced.md" >}})
- [Copyright]({{< relref "appendices/copyright.md" >}})
- [Disclaimer]({{< relref "appendices/disclaimer.md" >}})
- [Evolutions]({{< relref "appendices/evolutions/_index.md" >}})
  - [Evolutions of a Monolith that lead to Shards]({{< relref "appendices/evolutions/evolutions-of-a-monolith-that-lead-to-shards.md" >}})
  - [Evolutions of a Monolith that result in Layers]({{< relref "appendices/evolutions/evolutions-of-a-monolith-that-result-in-layers.md" >}})
  - [Evolutions of a Monolith that make Services]({{< relref "appendices/evolutions/evolutions-of-a-monolith-that-make-services.md" >}})
  - [Evolutions of a Monolith that rely on Plugins]({{< relref "appendices/evolutions/evolutions-of-a-monolith-that-rely-on-plugins.md" >}})
  - [Evolutions of Shards that share data]({{< relref "appendices/evolutions/evolutions-of-shards-that-share-data.md" >}})
  - [Evolutions of Shards that share logic]({{< relref "appendices/evolutions/evolutions-of-shards-that-share-logic.md" >}})
  - [Evolutions of Layers that make more layers]({{< relref "appendices/evolutions/evolutions-of-layers-that-make-more-layers.md" >}})
  - [Evolutions of Layers that help large projects]({{< relref "appendices/evolutions/evolutions-of-layers-that-help-large-projects.md" >}})
  - [Evolutions of Layers to improve performance]({{< relref "appendices/evolutions/evolutions-of-layers-to-improve-performance.md" >}})
  - [Evolutions of Layers to gain flexibility]({{< relref "appendices/evolutions/evolutions-of-layers-to-gain-flexibility.md" >}})
  - [Evolutions of Services that add or remove services]({{< relref "appendices/evolutions/evolutions-of-services-that-add-or-remove-services.md" >}})
  - [Evolutions of Services that add layers]({{< relref "appendices/evolutions/evolutions-of-services-that-add-layers.md" >}})
  - [Evolutions of a Pipeline]({{< relref "appendices/evolutions/evolutions-of-a-pipeline.md" >}})
  - [Evolutions of a Middleware]({{< relref "appendices/evolutions/evolutions-of-a-middleware.md" >}})
  - [Evolutions of a Shared Repository]({{< relref "appendices/evolutions/evolutions-of-a-shared-repository.md" >}})
  - [Evolutions of a Proxy]({{< relref "appendices/evolutions/evolutions-of-a-proxy.md" >}})
  - [Evolutions of an Orchestrator]({{< relref "appendices/evolutions/evolutions-of-an-orchestrator.md" >}})
  - [Evolutions of a Combined Component]({{< relref "appendices/evolutions/evolutions-of-a-combined-component.md" >}})
- [Format of a metapattern]({{< relref "appendices/format-of-a-metapattern.md" >}})
- [Glossary]({{< relref "appendices/glossary.md" >}})
- [History of changes]({{< relref "appendices/history-of-changes.md" >}})
- [Index of patterns]({{< relref "appendices/index-of-patterns.md" >}})

</nav>

<nav>

| \<\< [Index of patterns]({{< relref "appendices/index-of-patterns.md" >}}) |  | [Introduction]({{< relref "introduction/_index.md" >}}) \>\> |
| --- | --- | --- |

</nav>