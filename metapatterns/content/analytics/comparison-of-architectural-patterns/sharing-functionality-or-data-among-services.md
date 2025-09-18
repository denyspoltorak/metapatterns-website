+++
weight = 1
title = "Sharing functionality or data among services"
description = "Functionality or data may be shared among components through direct access, building a dedicated service, delegation, and replication."
images = ["/diagrams/Conclusion/Sharing-DirectCall.png"]
[sitemap]
  priority = 0.5
+++

# Sharing functionality or data among services

Architectural patterns manifest several ways of sharing functionality or data among their components\. Let’s consider a basic example: calls to two pieces of business logic need to be logged, while the logger is doing something more complex than mere console prints\. The business logic also needs to access a system\-wide counter\.

## Direct call

The simplest way to use a shared functionality \(aspect\) is to call the module which implements it directly\. This is possible if the users and the provider of the aspect reside in the same process, as in a [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) or module\-based \(single application\) [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}})\.

Sharing data inside a process is similar, but usually requires some kind of protection, like an [RW lock](https://en.wikipedia.org/wiki/Readers–writer_lock), around it to serialize access from multiple threads\.

<figure>
<a href="/diagrams/Conclusion/Sharing-DirectCall.png" style="outline:none">
<img src="/diagrams/Conclusion/Sharing-DirectCall.png" alt="Sharing-DirectCall" style="width:100%"/>
</a>
</figure>

## Make a dedicated service

In a distributed system you can place the functionality or data to share into a separate service to be accessed over the network, yielding [*Service\-Oriented Architecture*]({{< relref "../../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) for shared utilities or a [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) / [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) for shared data\.

<figure>
<a href="/diagrams/Conclusion/Sharing-DedicatedService.png" style="outline:none">
<img src="/diagrams/Conclusion/Sharing-DedicatedService.png" alt="Sharing-DedicatedService" style="width:100%"/>
</a>
</figure>

## Delegate the aspect

A less obvious solution is [delegating](https://datatracker.ietf.org/doc/html/rfc1925) our needs to another layer of the system\. To continue our example of logging, a [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) may log user requests and a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) – interservice communication\. In many cases one of these generic components is configurable to record all calls to the methods which we need to log – with no changes to the code\!

In a similar way a service may [behave as a function]({{< relref "../../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#functional-decentralized-streaming-paradigm--choreography" >}}): receive all the data it needs in an input message and send back all its work as an output – and let the database access remain the responsibility of its caller\.

<figure>
<a href="/diagrams/Conclusion/Sharing-Delegate.png" style="outline:none">
<img src="/diagrams/Conclusion/Sharing-Delegate.png" alt="Sharing-Delegate" style="width:100%"/>
</a>
</figure>

## Replicate it

Finally, each user of a component can get its own replica\. This is done implicitly in [*Shards*]({{< relref "../../basic-metapatterns/shards.md" >}}) and explicitly in a [*Service Mesh*]({{< relref "../../implementation-metapatterns/mesh.md#service-mesh" >}}) of [*Microservices*]({{< relref "../../basic-metapatterns/services.md#microservices" >}}) for libraries or [*Data Grid*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) of [*Space\-Based Architecture*]({{< relref "../../implementation-metapatterns/mesh.md#space-based-architecture" >}}) for data\.

Another case of replication is importing the same code in multiple services, which happens in [single\-layer *Nanoservices*]({{< relref "../../basic-metapatterns/services.md#inexact-nanoservices-api-layer" >}})\.

<figure>
<a href="/diagrams/Conclusion/Sharing-Duplicate.png" style="outline:none">
<img src="/diagrams/Conclusion/Sharing-Duplicate.png" alt="Sharing-Duplicate" style="width:100%"/>
</a>
</figure>

## Summary

There are four basic ways to share functionality or data in a system:

- Deploy everything together – messy but fast and simple\.
- Place the component in question into a shared service to be accessed over the network – slow and less reliable\.
- Let another layer of the system both implement and use the needed function on your behalf – easy but generic, thus it may not always fit your code’s needs\.
- Make a copy of the component for each of its users – fast, reliable, but the copies are hard to keep in sync\.


<nav>

| \<\< [Comparison of architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/_index.md" >}}) | ^ [Comparison of architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/_index.md" >}}) ^ | [Pipelines in architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) \>\> |
| --- | --- | --- |

</nav>