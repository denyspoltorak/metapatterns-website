+++
weight = 7
title = "Part 6. Analytics"
bookCollapseSection = true
+++

# Part 6\. Analytics

This part is dedicated to analyzing the architectural metapatterns, for if this book’s taxonomy of patterns is a step forward from the state of the art, it should bear fruits for us to pick\.

I had no time to research every idea collected while the book was being written and its individual chapters published for feedback\. A few of those pending topics, which may make additional chapters in the future, are listed below:

- Some architectural patterns \(*CQRS*, *Cache*, *Microservices*, etc\.\) appear under multiple metapatterns\. Each individual case makes a story of its own, teaching about both the needs of software systems and uses of metapatterns\.
- There are different ways to split a component into subcomponents: [*Layers of Services*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) differ from [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}})\. This should be investigated\.
- An architectural quality may depend on the structure of the system\. For example, a client request may be split into three subrequests to be processed sequentially or in parallel, depending on the topology \(e\.g\. [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}), [*Orchestrated Services*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) or [*Replicas*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}})\), thus it is topology which defines latency\. We can even draw formulas like:
  - L = L1 \+ L2 \+ L3 for *Pipeline*, 
  - L = MAX\(L1, L2, L3\) for *Orchestrated Services* which run in parallel,
  - L = MIN\(L1, L2, L3\) for *Replicas* with [*Request Hedging*](https://grpc.io/docs/guides/request-hedging/)\.


Other smaller topics that I was able to look into made the following chapters:

<nav>

| \<\< [Mesh]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) | ^ [Table of Contents]({{< relref "../_index.md" >}}) ^ | [Comparison of architectural patterns]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md" >}}) \>\> |
| --- | --- | --- |

</nav>



