+++
weight = 13
title = "Evolutions of a Pipeline"
description = "A Front Controller may be used to track states of running requests. Alternatively, adding an Orchestrator benefits complex scenarios."
images = ["/diagrams/Web/og/Favicon-plain.png"]
[sitemap]
  priority = 0.3
+++

# Evolutions of a Pipeline {anchor=false}

[*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}}) [inherits its set of evolutions from *Services*]({{< relref "../../basic-metapatterns/services.md#evolutions" >}})\. Components can be added, split in two, merged or replaced\. Many systems employ a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) \(pub/sub or pipeline framework\), [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) \(which may be a database or file system\) or [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}})\.

There are a couple of *Pipeline*\-specific evolutions:

- The first service of the *Pipeline* can be promoted to [*Front Controller*]({{< relref "../../extension-metapatterns/combined-component.md#front-controller" >}}) \[[SAHP]({{< relref "../../appendices/books-referenced.md#sahp" >}})\] which tracks status updates for every request it handles\.
- Adding an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) turns a [*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}}) into [*Services*]({{< relref "../../basic-metapatterns/services.md" >}})\. As the high\-level business logic moves to the orchestration layer, the services don’t need to interact directly, the interservice communication channels disappear and the system becomes identical to [*Orchestrated Services*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\.


## Promote a service to Front Controller

<figure>
<a href="/diagrams/Evolutions/Services/Pipeline%20promote%20Front%20Controller.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Pipeline%20promote%20Front%20Controller.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Pipeline%20promote%20Front%20Controller.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Pipeline%20promote%20Front%20Controller.png" alt="Pipeline promote Front Controller" loading="lazy" width="1299" height="347" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Front Controller]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \([Polyglot Persistence]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}), [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\), [Pipeline]({{< relref "../../basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../../basic-metapatterns/services.md" >}})\)\.

<ins>Goal</ins>: allow for clients to query the state of their requests\.

<ins>Prerequisite</ins>: request processing steps are slow \(may depend on human action\)\.

If request processing steps require heavy calculations or manual action, clients may want to query the status of their requests and analysts may want to see bottlenecks in the *Pipeline*\. Let the first service in the *Pipeline* track the state of all the running requests by subscribing to status notifications from other services\.

<ins>Pros</ins>: 

- The state of each running request is readily available\.


<ins>Cons</ins>: 

- The first service in the pipeline depends on every other service\.


<ins>Further steps</ins>:

- The *Front Controller* may be further promoted to [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) if there is a need to support many complex scenarios\.


## Add an Orchestrator

<figure>
<a href="/diagrams/Evolutions/Services/Pipeline%20use%20Orchestrator.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Pipeline%20use%20Orchestrator.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Pipeline%20use%20Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Pipeline%20use%20Orchestrator.png" alt="Pipeline use Orchestrator" loading="lazy" width="1303" height="384" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: support many use cases\.

<ins>Prerequisite</ins>: performance degradation is acceptable\.

When a [*choreographed*]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) system is extended with more and more use cases, it is very likely to fall into integration hell where nobody understands how its components interrelate\. Extract the workflow logic into a dedicated service\.

<ins>Pros</ins>: 

- New use cases are easy to add\.
- Complex scenarios are supported\.
- The services don’t depend on each other\.
- There is a single client\-facing team, other teams are not under pressure from the business\.
- It is easier to run actions in parallel\.
- Global scenarios become debuggable\.
- The services don’t need to be redeployed when the high\-level logic changes\.


<ins>Cons</ins>: 

- The number of messages in the system doubles, thus its performance may degrade\.
- The *Orchestrator* may become a development and performance bottleneck or a single point of failure\.


<ins>Further steps</ins>:

- If there are several clients that strongly vary in workflows, you can apply [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) with an *Orchestrator* per client\.
- If the *Orchestrator* grows too large, it can be [divided]({{< relref "../../extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers, services or both, the latter option resulting in a [*Top\-Down Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}})\.
- The *Orchestrator* can be [scaled]({{< relref "../../extension-metapatterns/orchestrator.md#scaled" >}}) and can have its own database\.
