+++
weight = 2
title = "Pipelines in architectural patterns"
description = "Pipeline is a unidirectional data flow. Depending on pattern, it may or may not preserve: data type, data identity and temporal order of the data stream."
images = ["/Conclusion/Pipelineliness-EventDrivenArchitecture.png"]
[sitemap]
  priority = 0.5
+++

# Pipelines in architectural patterns

Several architectural patterns involve a unidirectional data flow – a [*pipeline*](https://en.wikipedia.org/wiki/Pipeline_(software))\. Strictly speaking, every data packet in a pipeline should:

- Move through the system over the same *route* with no loops\.
- Be of the same *type*, making a *data stream*\.
- Retain its *identity* on the way\.
- Retain *temporal order* – the sequence of packets remains the same over the entire pipeline\.


Staying true to all of these points makes *Pipes and Filters* – one of the oldest known architectures\. Yet there are other architectures that discard one or more of the conditions:

## Pipes and Filters

<figure>
<a href="/diagrams/Conclusion/Pipelineliness-PipesAndFilters.png" style="outline:none">
<img src="/diagrams/Conclusion/Pipelineliness-PipesAndFilters.png" alt="Pipelineliness-PipesAndFilters" style="width:100%"/>
</a>
</figure>

[*Pipes and Filters*]({{< relref "../../basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}}) \[[POSA1]({{< relref "../../appendices/books-referenced.md#posa1" >}})\] is about stepwise [processing of a data stream]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}})\. Each piece of data \(a video frame, a line of text or a database record\) passes through the entire system\.

This architecture is easy to build and has a wide range of applications, from hardware to data analytics\. Though each pipeline is specialized for a single use case, a new one can often be built of the same set of generic components – this skill is mastered by Linux admins through their use of shell scripts\.

## Choreographed Event\-Driven Architecture

<figure>
<a href="/diagrams/Conclusion/Pipelineliness-EventDrivenArchitecture.png" style="outline:none">
<img src="/diagrams/Conclusion/Pipelineliness-EventDrivenArchitecture.png" alt="Pipelineliness-EventDrivenArchitecture" style="width:100%"/>
</a>
</figure>

Relaxing the *type* and loosening the *identity* clauses opens the way to [*Choreographed Event\-Driven Architecture*]({{< relref "../../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) \[[SAP]({{< relref "../../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../../appendices/books-referenced.md#fsa" >}})\], in which a service publishes notifications about anything it does that may be of interest to other services\. In such a system:

- There are multiple types of events going in different directions, like if several branched pipelines were built over the same set of services\.
- A service may aggregate multiple incoming events to publish a single, seemingly unrelated, event later when some condition is met\. For example, a warehouse delivery collects individual orders till it gets a truckload of them or until the evening comes and no new orders are accepted\.


This architecture covers way more complex use cases than [*Pipes and Filters*]({{< relref "../../basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}}) as multiple pipelines are present in the system and because processing an event is allowed to have loosely related consequences \(as with the parcel and truck\)\.

## Command Query Responsibility Segregation \(CQRS\)

<figure>
<a href="/diagrams/Conclusion/Pipelineliness-CQRS.png" style="outline:none">
<img src="/diagrams/Conclusion/Pipelineliness-CQRS.png" alt="Pipelineliness-CQRS" style="width:100%"/>
</a>
</figure>

When data from events is stored for a future use \(as with the aggregation above\), the *type* and *temporal order* are ignored but data *identity* may be retained\. A [*CQRS*\-based system]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] separates paths for write \(*command*\) and read \(*query*\) requests, making a kind of data processing pipeline with the database in the middle, which stores events for an indeterminate amount of time\. It is the database that reshuffles the order of events, as a record it stores may be queried at any time, maybe in a year from its addition – or never at all\.

## Model\-View\-Controller \(MVC\)

<figure>
<a href="/diagrams/Conclusion/Pipelineliness-MVC.png" style="outline:none">
<img src="/diagrams/Conclusion/Pipelineliness-MVC.png" alt="Pipelineliness-MVC" style="width:100%"/>
</a>
</figure>

[*Model\-View\-Controller*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) \[[POSA1]({{< relref "../../appendices/books-referenced.md#posa1" >}})\] completely neglects the *type* and *identity* limitations\. It is a coarse\-grained pattern where the input source produces many kinds of events that go to the main module which does something and outputs another stream of events of no obvious relation to the input\. A mouse click does not necessarily result in a screen redraw, while a redraw may happen on timer with no user actions\. In fact, the pattern conjoins two different short pipelines\.

## Summary

There are four architectures with unidirectional data flow, which is characteristic of [*pipelines*]({{< relref "../../basic-metapatterns/pipeline.md" >}}):

- [*Pipes and Filters*]({{< relref "../../basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}}),
- [*Choreographed Event\-Driven Architecture* \(*EDA*\)]({{< relref "../../basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}),
- [*Command \(and\) Query Responsibility Segregation* \(*CQRS*\)]({{< relref "../../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}),
- [*Model\-View\-Controller* \(*MVC*\)]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}})\.


The first two, being true pipelines, are built around data processing and transformation, while for the others it is just an aspect of implementation – their separation of input and output yields pairs of streams\.

<nav>

| \<\< [Sharing functionality or data among services]({{< relref "../../analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}) | ^ [Comparison of architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/_index.md" >}}) ^ | [Dependency inversion in architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/dependency-inversion-in-architectural-patterns.md" >}}) \>\> |
| --- | --- | --- |

</nav>