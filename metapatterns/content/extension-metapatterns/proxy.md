+++
weight = 8
title = "Proxy"
description = "A Proxy represents a system to its clients. It implements cross-cutting concerns, such as caching or protocol translation, and routes client requests."
+++

# Proxy

<figure style="text-align:center">
<a href="/Main/Proxy.png" style="outline:none">
<img src="/Main/Proxy.png" alt="Proxy" style="width:100%"/>
</a>
</figure>

*Should I build the wall?* A layer of indirection between your system and its clients\.

<ins>Known as:</ins> Proxy \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\]\.

<ins>Aspects:</ins>

- Routing,
- Offloading\.


<ins>Variants:</ins>

By [transparency](https://community.f5.com/kb/technicalarticles/what-is-a-proxy/282718):

- Full Proxy,
- Half\-Proxy\.


By placement:

- Separate deployment,
- On the system side: Sidecar \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\],
- On the client side: Ambassador \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]\.


By function:

- Firewall / [\(API\) Rate Limiter](https://testfully.io/blog/api-rate-limit/) / API Throttling,
- Response Cache / [Read\-Through Cache](https://www.enjoyalgorithms.com/blog/read-through-caching-strategy) / [Write\-Through Cache](https://www.enjoyalgorithms.com/blog/write-through-caching-strategy) / [Write\-Behind Cache](https://www.enjoyalgorithms.com/blog/write-behind-caching-pattern) / Cache \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / Caching Layer \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / Distributed Cache / Replicated Cache,
- [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / Sharding Proxy \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / [Cell Router](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/cell-routing.html) / Messaging Grid \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] / Scheduler, 
- Dispatcher \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] / [Reverse Proxy](https://traefik.io/blog/reverse-proxy-vs-ingress-controller-vs-api-gateway/) / [Ingress Controller](https://traefik.io/blog/reverse-proxy-vs-ingress-controller-vs-api-gateway/) / [Edge Service](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41) / [Microgateway](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md), 
- Adapter \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}}), [DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] / Anticorruption Layer \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] / Open Host Service \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] / Gateway \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] / Message Translator \[[EIP]({{< relref "../appendices/books-referenced.md#eip" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] / [API Service](https://backendless.com/what-is-api-as-a-service/) / [Cell Gateway](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) / \(inexact\) Backend for Frontend / [Hardware Abstraction Layer](https://en.wikipedia.org/wiki/Hardware_abstraction) \(HAL\) / [Operating System Abstraction Layer](https://en.wikipedia.org/wiki/Operating_system_abstraction_layer) \(OSAL\) / Platform Abstraction Layer \(PAL\) / [Database Abstraction Layer](https://en.wikipedia.org/wiki/Database_abstraction_layer) \(DBAL or DAL\) / Database Access Layer \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] / Data Mapper \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] / [Repository](https://martinfowler.com/eaaCatalog/repository.html) \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}}), [DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\.
- \(with [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\) API Gateway \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\]\. 


See also [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \(a *Gateway* per client type\)\.

<ins>Structure:</ins> A layer that pre\-processes and/or routes user requests\.

<ins>Type:</ins> Extension\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Separates cross\-cutting concerns from the services | A single point of failure |
| Decouples the system from its clients | Most proxies degrade latency |
| Low attack surface |  |
| Available off the shelf |  |


<ins>References:</ins> Half of \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] is about the use of *Proxies*\. See also: \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] on *Proxy*; [Chris Richardson](https://microservices.io/patterns/apigateway.html) and [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway) on *API Gateway*; [Martin Fowler](https://martinfowler.com/articles/gateway-pattern.html) on *Gateway*, *Facade* and *API Gateway*\.

A *Proxy* stands between a \(sub\)system’s implementation and its users\. It receives a request from a client, does some pre\-processing, then forwards the request to a lower\-level component\. In other words, a *Proxy* encapsulates selected aspects of the system’s communication with its clients by serving as yet another layer of indirection\. It may also decouple the system’s internals from changes in the public protocol\. The [main functions](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway) of a proxy include:

- *Routing* – a *Proxy* tracks addresses of deployed instances of the system’s components and is able to forward a client’s request to the [*shard*]({{< relref "../basic-metapatterns/shards.md" >}}) or [*service*]({{< relref "../basic-metapatterns/services.md" >}}) which can handle it\. Clients need to know only the public address of the *Proxy*\. A *Proxy* may also respond on its own if the request is invalid or there is a matching response in the *Proxy*’s cache\.
- *Offloading* – a *Proxy* may implement generic aspects \([*cross\-cutting concerns*](https://en.wikipedia.org/wiki/Cross-cutting_concern)\) of the system’s public interface, such as authentication, authorisation, encryption, request logging, web protocol support, etc\. which would otherwise need to be implemented by the underlying system components\. That allows for the services to concentrate on what you write them for – the business logic\.


### Performance

Most kinds of proxies trade latency \(the extra network hop\) for some other quality:

- A [*Firewall*]({{< relref "#firewall-api-rate-limiter-api-throttling" >}}) slows down processing of good requests but *protects* the system from attacks\.
- Both a [*Load Balancer*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) and a [*Dispatcher*]({{< relref "#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}) allow for the use of multiple servers \(with identical or specialized components, correspondingly\) to improve the system’s *throughput* but they still add to the minimum latency\.
- An [*Adapter*]({{< relref "#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) adds *compatibility* but its latency cost is higher than with other *Proxies* as it not only forwards the original message but also changes its payload – an activity which involves data processing and serialization\.


A [*Cache*]({{< relref "#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) is a bit weird in that respect\. It improves latency and throughput for repeated requests but degrades latency for unique ones\. Furthermore, it is often colocated with some other kind of *Proxy* to avoid the extra network hop between the *Proxies*, which makes caching almost free in terms of latency\.

### Dependencies

*Proxies* widely vary in their functionality and level of intrusiveness\. The most generic proxies, like *Firewalls*, may not know anything about the system or its clients\. A *Response Cache* or *Adapter* must parse incoming messages, thus it depends on the communication protocol and message format\. A *Load Balancer* or *Dispatcher* is aware of both the protocol and system composition\.

In fact, because *Proxies* tend to have their dependencies configured on startup or through their APIs, there is no need to modify the code of a *Proxy* each time something changes in the underlying system\.

### Applicability

*Proxy* <ins>helps</ins> with:

- *Multi\-component systems\.* Having multiple types and/or instances of services means there is a need to know the components’ addresses to access them\. A *Proxy* encapsulates that knowledge and may also provide other common functionality as an extra benefit\.
- *Dynamic scaling or sharding\.* The *Proxy* both knows the system’s structure \(the address of each instance of a service\) and delivers user requests, thus it is the place to implement *sharding* \(when a service instance is [dedicated to a subset of users]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}})\) or *load balancing* \(when any service instance can [serve any user]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}})\) and even manage the size of the service instance *Pool*\.
- *Multiple client protocols*\. When the *Proxy* is the endpoint for the system’s users it may translate multiple external \(user\-facing\) protocols into a unified internal representation\.
- *System security\.* Though a *Proxy* does not make a system more secure, it takes away the burden of security considerations from the services which implement the business logic, improving the separation of concerns and making the system components more simple and stupid\. An off\-the\-shelf *Proxy* may be less vulnerable compared to in\-house services \(but don’t disregard [security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity)\!\)\.


*Proxy* <ins>hurts</ins>:

- *Critical real\-time paths\.* It adds an extra hop in request processing, increasing latency for thoroughly optimized use cases\. Such requests may need to bypass *Proxies*\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Proxy.png" style="outline:none">
<img src="/Relations/Proxy.png" alt="Proxy" style="width:100%"/>
</a>
</figure>

*Proxy*:

- Extends [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) \(forming *Layers*\), [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}), or [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- Can be extended by another *Proxy* or merged with an [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) into an [*API Gateway*]({{< relref "../extension-metapatterns/combined-component.md#api-gateway" >}})\.
- At least one *Proxy* per [*service*]({{< relref "../basic-metapatterns/services.md" >}}) is employed by [*Message Bus*]({{< relref "../extension-metapatterns/combined-component.md#message-bus" >}}), [*Enterprise Service Bus*]({{< relref "../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}), [*Service Mesh*]({{< relref "../implementation-metapatterns/mesh.md#service-mesh" >}}), and [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})\.
- Is a special case \(when there is a single kind of client\) of [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


## Variants by transparency

A *Proxy* [may either fully isolate the system it represents or merely help establish connections](https://community.f5.com/kb/technicalarticles/what-is-a-proxy/282718) between clients and servers\. This resembles [closed and open layers]({{< relref "../basic-metapatterns/layers.md#dependencies" >}}) because a *Proxy* is a [layer]({{< relref "../basic-metapatterns/layers.md" >}}) between a system and its clients\.

### Full Proxy

<figure style="text-align:center">
<a href="/Variants/2/Full%20Proxy.png" style="outline:none">
<img src="/Variants/2/Full%20Proxy.png" alt="Full Proxy" style="width:94%"/>
</a>
</figure>

A *Full Proxy* processes every message between the system and its clients\. It completely isolates the system and may meddle with the protocols but it is resource\-heavy and adds to latency\. [*Adapters*]({{< relref "#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) and [*Response Caches*]({{< relref "#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}) are always *Full Proxies*\.

### Half\-Proxy

<figure style="text-align:center">
<a href="/Variants/2/Half%20Proxy.png" style="outline:none">
<img src="/Variants/2/Half%20Proxy.png" alt="Half Proxy" style="width:100%"/>
</a>
</figure>

A *Half\-Proxy* intercepts, analyzes, and routes the session establishment request from a client but then goes out of the loop\. It may still forward the subsequent messages without looking into their content or it may even help connect the client and server directly, which is known as [*direct server return*](https://www.haproxy.com/glossary/what-is-direct-server-return-dsr) *\(DSR\)*\. This approach is faster and much less resource\-hungry but is also less secure and less flexible than that of *Full Proxy*\. A [*Firewall*]({{< relref "#firewall-api-rate-limiter-api-throttling" >}}), [*Load Balancer*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), or [*Reverse Proxy*]({{< relref "#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}) may act as a *Half\-Proxy*\. IP telephony servers often use *DSR*: the server helps call parties find each other and establish direct media communication\.

## Variants by placement

As a *Proxy* stands between a \(sub\)system and its client\(s\), we can imagine a few ways to deploy it:

### Separate deployment: Standalone

<figure style="text-align:center">
<a href="/Variants/2/Proxy%20placement%20-%20Standalone.png" style="outline:none">
<img src="/Variants/2/Proxy%20placement%20-%20Standalone.png" alt="Proxy placement - Standalone" style="width:100%"/>
</a>
</figure>

We can deploy a *Proxy* as a separate system component\. This has the downside of an extra network hop \(higher latency\) in the way of every client’s request to the system and back but that is unavoidable in the following cases:

- The *Proxy* uses a lot of system resources, thus it cannot be colocated with another component\. This mostly affects [*Firewall*]({{< relref "#firewall-api-rate-limiter-api-throttling" >}}) and [*Cache*]({{< relref "#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}})\.
- The *Proxy* is stateful and deals with multiple services, which is true for a [*Load Balancer*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), [*Reverse Proxy*]({{< relref "#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), or [*API Gateway*]({{< relref "#api-gateway" >}})\.


### On the system side: Sidecar

<figure style="text-align:center">
<a href="/Variants/2/Proxy%20placement%20-%20Sidecar.png" style="outline:none">
<img src="/Variants/2/Proxy%20placement%20-%20Sidecar.png" alt="Proxy placement - Sidecar" style="width:100%"/>
</a>
</figure>

We can often co\-locate a *Proxy* with our system when the latter is not distributed\. That avoids the extra network delay, traffic, operational complexity and does not add any new hardware which can fail at the most untimely moments\. Such a placement is called *Sidecar* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] and it is mostly applicable to [*Adapters*]({{< relref "#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}})\.

It should be noted that *Sidecar* – co\-locating a generic component and business logic – is more of a DevOps approach than an architectural pattern, thus we can see it used in a variety of ways \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]:

- As a *Proxy* between a component and its clients\.
- As an extra [*service*]({{< relref "../basic-metapatterns/services.md" >}}) that provides observability or configures the main service\.
- As a [*layer*]({{< relref "../basic-metapatterns/layers.md" >}}) with general\-purpose utilities\.
- As an *Adapter* for [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}})\.


<figure style="text-align:center">
<a href="/Variants/2/Sidecars.png" style="outline:none">
<img src="/Variants/2/Sidecars.png" alt="Sidecars" style="width:100%"/>
</a>
</figure>

[*Service Mesh*]({{< relref "../implementation-metapatterns/mesh.md#service-mesh" >}}) \([*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) for [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}})\) makes heavy use of *Sidecars*\.

### On the client side: Ambassador

<figure style="text-align:center">
<a href="/Variants/2/Proxy%20placement%20-%20Ambassador.png" style="outline:none">
<img src="/Variants/2/Proxy%20placement%20-%20Ambassador.png" alt="Proxy placement - Ambassador" style="width:100%"/>
</a>
</figure>

Finally, a *Proxy* may be co\-located with a component’s clients, making it an *Ambassador*  \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]\. Its use cases include:

- Low\-latency systems with [stateful *shards*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) – each client should access the shard that has their data, which the *Proxy* knows how to choose\.
- [*Adapters*]({{< relref "#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) that help client applications use an optimized or secure protocol\.


## Variants by function

*Proxies* are ubiquitous in backend systems as using one or several of them frees the underlying code from the need to provide boilerplate non\-business\-logic functionality\. It is common to have several kinds of *Proxies* deployed sequentially \(e\.g\. [*API Gateways*]({{< relref "#api-gateway" >}}) behind [*Load Balancers*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) behind a [*Firewall*]({{< relref "#firewall-api-rate-limiter-api-throttling" >}})\) with many of them [*pooled*]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) to improve performance and stability\. It is also possible to employ multiple kinds of *Proxies*, each serving its own kind of client, in parallel, resulting in [*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

As *Proxies* are used for many purposes, there are a variety of their specializations and names\. Below is a very rough categorization, complicated by the fact that real\-world *Proxies* often implement several categories at once\.

<aside>

> For example, NGINX claims to be: an HTTP web server, [*Reverse Proxy*]({{< relref "#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), content [*Cache*]({{< relref "#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Load Balancer*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), TCP/UDP *Proxy* server, and mail *Proxy* server – all at once\.

</aside>

### Firewall, \(API\) Rate Limiter, API Throttling

<figure style="text-align:center">
<a href="/Variants/2/Firewall.png" style="outline:none">
<img src="/Variants/2/Firewall.png" alt="Firewall" style="width:92%"/>
</a>
</figure>

The *Firewall* is a component for white\- and black\-listing network traffic, mostly to protect against attacks\. It is possible to use both generic hardware *Firewalls* on the external perimeter for brute force \(D\)DoS protection and more complex access rules at a second layer of software *Firewalls* to protect critical data and services from unauthorized access\.

[*Rate Limiting*](https://testfully.io/blog/api-rate-limit/) makes sure that no single client uses too much of the system’s resources – it sets a limit on how many requests from a single source the system can process over a unit of time\. Any requests over the limit are rejected\.

*Throttling* differs from *Rate Limiting* in that over\-the\-limit requests are queued for later processing, effectively slowing down communication with aggressive clients\.

### Response Cache, Read\-Through Cache, Write\-Through Cache, Write\-Behind Cache, Cache, Caching Layer, Distributed Cache, Replicated Cache

<figure style="text-align:center">
<a href="/Variants/2/Cache.png" style="outline:none">
<img src="/Variants/2/Cache.png" alt="Cache" style="width:100%"/>
</a>
</figure>

If a system often gets identical requests, it is possible to remember its responses to most frequent of them and return the cached response without fully re\-processing the request\. The real thing is more complicated because users tend to change the data which the system stores, necessitating a variety of *cache refresh policies*\. A *Response Cache* may be co\-located with a *Load Balancer* or it may be \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] [*sharded*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) \(each *Cache* processes a unique subset of requests\) and/or [*replicated*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) \(all the *Caches* are similar\) and thus require a *Load Balancer* of its own\.

It is called *Response Cache* because it stores the system’s responses to requests of its users or just *Cache* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] because it is the most common kind of *Cache* in system architecture\.

If the cached subsystem is a database, we can discern between read and write requests:

- [*Read\-Through Cache*](https://www.enjoyalgorithms.com/blog/read-through-caching-strategy) is when the *Cache* is updated on a miss for a read request but is transparent to or invalidated by write requests\.
- [*Write\-Through Cache*](https://www.enjoyalgorithms.com/blog/write-through-caching-strategy) is when the *Cache* is updated by write requests that pass through it\.
- [*Write\-Behind*](https://www.enjoyalgorithms.com/blog/write-behind-caching-pattern) is when the *Cache* aggregates multiple write requests to later send them to the database as a batch, saving bandwidth and possibly merging multiple updates of the same key\.


It is possible to combine multiple servers into a virtual *Caching Layer* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]:

- In the simplest case, which does not require any additional instrumentation aside from a [*Load Balancer*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), the instances of the *Caches* are independent and may return stale results\.
- In a *Distributed Cache*, driven by a [*Sharding Proxy*]({{< relref "#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}), every server \([*shard*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}})\) holds a subset of the cached data, thus allowing for caching datasets which don’t fit in a single computer’s memory\.
- In a *Replicated Cache* the datasets of all the servers are identical and synchronized on any modification\. This scales the cache’s throughput but requires a kind of synchronization engine, like a [*Data Grid*]({{< relref "../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.


### Load Balancer, Sharding Proxy, Cell Router, Messaging Grid, Scheduler

<figure style="text-align:center">
<a href="/Variants/2/Load%20Balancer.png" style="outline:none">
<img src="/Variants/2/Load%20Balancer.png" alt="Load Balancer" style="width:100%"/>
</a>
</figure>

Here we have a hardware or software component which distributes user traffic among multiple [instances]({{< relref "../basic-metapatterns/shards.md" >}}) of a service:

- A *Sharding Proxy* \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] selects a [*shard*]({{< relref "../basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) based on specific data which is present in a request \(OSI level 7 request routing\) for a system where each shard owns a part of the system’s state, therefore only one \(or a few for [*replicated*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}) *shards*\) of the shards has the data required to process the client’s request\.
- A [*Load Balancer*](https://en.wikipedia.org/wiki/Load_balancing_(computing)) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] for a [*Pool of stateless instances*]({{< relref "../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) or [*Replicas*]({{< relref "../basic-metapatterns/shards.md#persistent-copy-replica" >}}), or a *Messaging Grid* \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\] of [*Space\-Based Architecture*]({{< relref "../extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) evenly distributes the incoming traffic over identical request processors \([OSI level](https://en.wikipedia.org/wiki/OSI_model) 4 load balancing\) to protect any instance of the underlying system from overload\. In some cases it needs to be session\-aware \(process OSI level 7\) to assure that all the requests from a client are forwarded to the same instance of the service \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\]\.
- It may forward read requests to [*Read\-Only Replicas*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#read-only-replica" >}}) of the data while write requests are sent to the *master* database \([*CQRS*](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)\-like behavior\)\.
- A [*Cell Router*](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/cell-routing.html) chooses a data center which is the closest to the user’s location\.


*Load Balancers* are very common in high\-load backends\. High\-availability systems deploy multiple instances of a *Load Balancer* in parallel to remain functional if one of the *Load Balancers* fails\. CPU\-intensive applications \(like 3D games\) often post asynchronous tasks for execution by *Thread Pools* under the supervision of a *Scheduler*\. A similar pattern is found in OS kernels and *fiber* or *actor* frameworks where a limited set of CPU\-affined threads is scheduled to run a much larger number of tasks\.

### Dispatcher, Reverse Proxy, Ingress Controller, Edge Service, Microgateway

<figure style="text-align:center">
<a href="/Variants/2/Dispatcher.png" style="outline:none">
<img src="/Variants/2/Dispatcher.png" alt="Dispatcher" style="width:100%"/>
</a>
</figure>

The [*Reverse Proxy*, *Ingress Controller*](https://traefik.io/blog/reverse-proxy-vs-ingress-controller-vs-api-gateway/), [*Edge Service*](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41), or [*Microgateway*](https://github.com/wso2/reference-architecture/blob/master/event-driven-api-architecture.md) is a router that stands between the Internet and the organization’s internal network\. It allows clients to use a public address for the system without knowing how and where their requests are processed\. It parses user requests and forwards them to an internal server based on the requests’ body\. A *Reverse Proxy* can be extended with a *firewall*, *SSL termination*, *load balancing*, and *caching* functionality\. Examples include Nginx\.

*Dispatcher* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] is a similar component for a single\-process application\. It serves a complex command line interface by receiving and preprocessing user commands only to forward each command to a module which knows how to handle it\. The modules may register their commands with the *Dispatcher* at startup or there may be a static dispatch table in the code\.

You could have noticed that *Dispatcher* or *Reverse Proxy* is quite similar to *Load Balancer* or *Sharding Proxy* – they differ mostly in what kind of system lies below them: [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) or [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}})\.

### Adapter, Anticorruption Layer, Open Host Service, Gateway, Message Translator, API Service, Cell Gateway, \(inexact\) Backend for Frontend, Hardware Abstraction Layer \(HAL\), Operating System Abstraction Layer \(OSAL\), Platform Abstraction Layer \(PAL\), Database Abstraction Layer \(DBAL or DAL\), Database Access Layer, Data Mapper, Repository

<figure style="text-align:center">
<a href="/Variants/2/Adapter.png" style="outline:none">
<img src="/Variants/2/Adapter.png" alt="Adapter" style="width:100%"/>
</a>
</figure>

An *Adapter* \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}}), [DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] is a mostly stateless *Proxy* that translates between an internal and public protocol and API formats\. It may often be co\-located with a *Reverse Proxy*\. When it adapts messages, it is called a *Message Translator* \[[EIP]({{< relref "../appendices/books-referenced.md#eip" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\]\.

As an *Adapter* adapts in two directions, it is often found between two components \(in [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})\) or between a component and [*Middleware*]({{< relref "../extension-metapatterns/middleware.md" >}}) \(in [*Enterprise Service Bus*]({{< relref "../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) and [*Service Mesh*]({{< relref "../extension-metapatterns/combined-component.md#service-mesh" >}})\)\.

In \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\], when one component \(*consumer*\) depends on another \(*supplier*\), there may be an *Adapter* in between to decouple them\. It is called *Anticorruption Layer* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] when owned by the *consumer*’s team or *Open Host Service* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] if the *supplier* adds it to grant one or more stable interfaces \(*Published Languages* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\)\.

A *Gateway* \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] or [*API Service*](https://backendless.com/what-is-api-as-a-service/) often implies an *Adapter* with extra functionality, like *Reverse Proxy*, authorization and authentication\. [*Cell Gateway*](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) is a *Gateway* for a [*Cell*]({{< relref "../basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.

When a *Gateway* translates a single public API method into several calls towards internal services, it becomes an *API Gateway* \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] which is an aggregate of *Proxy* \(for protocol translation\) and [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\.

An *Adapter* between an end\-user client \(web interface, mobile application, etc\.\) and the system’s API is often called [*Backend for Frontend*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\. It decouples the UI from the backend\-owned system’s API, giving the teams behind both components freedom to work with less synchronization\.

There is also a whole bunch of *Adapters* that aim to protect the business logic from its environment, the idea which is perfected by [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}}):

- [*Hardware Abstraction Layer*](https://en.wikipedia.org/wiki/Hardware_abstraction) \(*HAL*\) hides details of hardware to make the code portable\.
- [*Operating System Abstraction Layer*](https://en.wikipedia.org/wiki/Operating_system_abstraction_layer) \(*OSAL*\) or *Platform Abstraction Layer* \(*PAL*\) abstracts the OS to make the application cross\-platform\.
- [*Database Abstraction Layer*](https://en.wikipedia.org/wiki/Database_abstraction_layer) \(*DBAL* or *DAL*\), *Database Access Layer* \[[POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] or *Data Mapper* \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}})\] attempts to help building database\-agnostic applications by making all the databases look the same\.
- [*Repository*](https://martinfowler.com/eaaCatalog/repository.html) \[[PEAA]({{< relref "../appendices/books-referenced.md#peaa" >}}), [DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] provides methods to access a record stored in a database as if it were an object in the application’s memory\.


<aside>

> An *Adapter* creates a [layer of indirection](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering) between your code and a library or service which it uses\. If the external component’s interface changes, or you need to substitute the thing with an incompatible implementation from another vendor, and your code accesses the component directly, you will have to make many changes throughout your code\. However, if there is an *Adapter* in\-between, your code depends only on the interface of the *Adapter*\. And when the external component changes or is replaced, only the relatively small *Adapter*’s implementation needs to change while your main code is blessed with ignorance of what lies beyond the *Adapter*’s borders\. 

</aside>

### API Gateway

<figure style="text-align:center">
<a href="/Variants/2/API%20Gateway.png" style="outline:none">
<img src="/Variants/2/API%20Gateway.png" alt="API Gateway" style="width:100%"/>
</a>
</figure>

*API Gateway* \[[MP]({{< relref "../appendices/books-referenced.md#mp" >}})\] is a fusion of [*Gateway*]({{< relref "#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(*Proxy*\) and [*API Composer*]({{< relref "../extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) \([*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\)\. The *Gateway* aspect encapsulates the external \(public\) protocol while the *API Compose*r translates the system’s high\-level public API methods into multiple \(usually parallel\) calls to the APIs of internal components, collects the results and conjoins them into a response\.

*API Gateway* is [discussed in more detail]({{< relref "../extension-metapatterns/orchestrator.md#api-gateway" >}}) under *Orchestrator*\.

## Evolutions

It usually makes little sense to get rid of a *Proxy* once it has been integrated into a system\. The only real drawback to using a *Proxy* is a slight increase in latency for user requests which may be mitigated through the creation of [bypass channels]({{< relref "#half-proxy" >}}) between the clients and a service that needs low latency\. The other drawback of the pattern, the *Proxy* being a single point of failure, is countered by deploying multiple instances of the *Proxy*\.

As *Proxies* are usually third\-party products, there is not much [we can change about them]({{< relref "../appendices/evolutions/evolutions-of-a-proxy.md" >}}):

- We can add another kind of a *Proxy* on top of an existing one\.


<figure style="text-align:center">
<a href="/Evolutions/2/Proxy%20add%20Proxy.png" style="outline:none">
<img src="/Evolutions/2/Proxy%20add%20Proxy.png" alt="Proxy add Proxy" style="width:100%"/>
</a>
</figure>

- We can use a stack of *Proxies* per client, making *Backends for Frontends*\.


<figure style="text-align:center">
<a href="/Evolutions/2/Proxy%20to%20Backends%20for%20Frontends.png" style="outline:none">
<img src="/Evolutions/2/Proxy%20to%20Backends%20for%20Frontends.png" alt="Proxy to Backends for Frontends" style="width:100%"/>
</a>
</figure>

## Summary

 A *Proxy* represents your system to its clients and takes care of some aspects of the communication\. It is common to see multiple *Proxies* deployed sequentially as they are often stackable\.

<nav>

| \<\< [Shared Repository]({{< relref "../extension-metapatterns/shared-repository.md" >}}) | ^ [Extension metapatterns]({{< relref "../extension-metapatterns/_index.md" >}}) ^ | [Orchestrator]({{< relref "../extension-metapatterns/orchestrator.md" >}}) \>\> |
| --- | --- | --- |

</nav>



