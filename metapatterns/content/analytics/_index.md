+++
weight = 7
title = "Analytics"
description = "This part compares different aspects of patterns which were covered in the previous chapters and outlines a few general topics of software architecture."
images = ["/diagrams/Web/og/Favicon-plain.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Analytics {anchor=false}

This part is dedicated to analyzing the architectural [metapatterns]({{< relref "../introduction/metapatterns.md" >}}), for if this book’s taxonomy of patterns is a step forward from the state of the art, it should bear fruits for us to pick\.

I had no time to research every idea collected while the book was being written and its individual chapters published for feedback\. A few of those pending topics, which may make additional chapters in the future, are listed below:

- Some architectural patterns \(*CQRS*, *Cache*, *Microservices*, etc\.\) appear under multiple metapatterns\. Each individual case makes a story of its own, teaching about both the needs of software systems and uses of metapatterns\.
- There are different ways to split a component into subcomponents: [*Layers of Services*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) differ from [*Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md" >}})\. This should be investigated\.
- An architectural quality may depend on the structure of the system\. For example, a client request may be split into three subrequests to be processed sequentially or in parallel, depending on the topology \(e\.g\. [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}), [*Orchestrated Services*]({{< relref "../extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) or [*Replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}})\), thus it is topology which defines latency\. We can even draw formulas like:
  - L = L1 \+ L2 \+ L3 for *Pipeline*, 
  - L = MAX\(L1, L2, L3\) for *Orchestrated Services* which run in parallel,
  - L = MIN\(L1, L2, L3\) for *Replicas* with [*Request Hedging*](https://grpc.io/docs/guides/request-hedging/)\.


Other smaller topics that I was able to look into made the following chapters:

## Contents:

<nav class="grid3">

<a href="{{< relref "analytics/comparison-of-architectural-patterns/_index.md" >}}">

<picture>

<source srcset="/diagrams/Web/Comparison.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Comparison.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Comparison.png" alt="" loading="lazy" width="665" height="645"/>

</picture>

Comparison of architectural patterns

</a>

<a href="{{< relref "analytics/ambiguous-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Ambiguous.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Ambiguous.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Ambiguous.png" alt="" loading="lazy" width="1003" height="983"/>

</picture>

Ambiguous patterns

</a>

<a href="{{< relref "analytics/architecture-and-product-life-cycle.md" >}}">

<picture>

<source srcset="/diagrams/Web/Life%20cycle.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Life%20cycle.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Life%20cycle.png" alt="" loading="lazy" width="1185" height="1123"/>

</picture>

Architecture and product life cycle

</a>

<a href="{{< relref "analytics/real-world-inspirations-for-architectural-patterns.md" >}}">

<picture>

<source srcset="/diagrams/Web/Real-world.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Real-world.negated.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Real-world.png" alt="" loading="lazy" width="483" height="463"/>

</picture>

Real-world inspirations for architectural patterns

</a>

<a href="{{< relref "analytics/the-heart-of-software-architecture/_index.md" >}}">

<picture>

<source srcset="/diagrams/Web/Heart.svg" media="(prefers-color-scheme: light)"/>

<source srcset="/diagrams/Web/Heart.dark.svg" media="(prefers-color-scheme: dark)"/>

<img src="/diagrams/Web/Heart.png" alt="" loading="lazy" width="403" height="384"/>

</picture>

The heart of software architecture

</a>

</nav>