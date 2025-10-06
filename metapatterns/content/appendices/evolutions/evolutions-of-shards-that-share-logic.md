+++
weight = 6
title = "Evolutions of Shards that share logic"
description = "Intershard communication is helped by a Middleware or is made unnecessary by employing a Sharding Proxy or an Orchestrator."
[sitemap]
  priority = 0.3
+++

# Evolutions of Shards that share logic

Other cases are better solved by extracting the logic that manipulates multiple [*shards*]({{< relref "../../basic-metapatterns/shards.md" >}}):

- Splitting a [*service*]({{< relref "../../basic-metapatterns/services.md" >}}) \(as discussed [above]({{< relref "../../appendices/evolutions/evolutions-of-shards-that-share-data.md#split-a-service-with-the-coupled-data" >}})\) yields a component that represents both shared data and shared logic\.
- Adding a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) lets the shards communicate with each other without keeping direct connections\. It also may do housekeeping: error recovery, replication, and scaling\.
- A [*Sharding Proxy*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) hides the existence of the shards from clients\.
- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) calls \(or messages\) multiple shards to serve a user request\. That relieves the shards of the need to coordinate their states and actions by themselves\.


## Add a Middleware

<figure>
<a href="/diagrams/Evolutions/Shards/Shards%20add%20Middleware.png">
<picture>
<source srcset="/diagrams/Evolutions/Shards/Shards%20add%20Middleware.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Shards/Shards%20add%20Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Shards/Shards%20add%20Middleware.png" alt="Shards add Middleware" loading="lazy" width="1065" height="324" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Shards]({{< relref "../../basic-metapatterns/shards.md" >}}), [Middleware]({{< relref "../../extension-metapatterns/middleware.md" >}}), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: simplify communication between shards, their deployment, and recovery\.

<ins>Prerequisite</ins>: many shards need to exchange information, some may fail\.

A *Middleware* transports messages between shards, checks their health and recovers ones which have crashed\. It may manage data replication and deployment of software updates as well\.

<ins>Pros</ins>: 

- The shards become simpler because they don’t need to track each other\.
- There are many good third\-party implementations\.


<ins>Cons</ins>: 

- Performance may degrade\.
- Components of the *Middleware* are new points of failure\.


## Add a Sharding Proxy

<figure>
<a href="/diagrams/Evolutions/Shards/Shards%20add%20Load%20Balancer.png">
<picture>
<source srcset="/diagrams/Evolutions/Shards/Shards%20add%20Load%20Balancer.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Shards/Shards%20add%20Load%20Balancer.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Shards/Shards%20add%20Load%20Balancer.png" alt="Shards add Load Balancer" loading="lazy" width="1026" height="423" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Shards]({{< relref "../../basic-metapatterns/shards.md" >}}), [Sharding Proxy]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) \([Proxy]({{< relref "../../extension-metapatterns/proxy.md" >}})\), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: simplify the code on the client side, hide your implementation from clients\.

<ins>Prerequisite</ins>: each client connects directly to the shard which owns their data\.

The client application may know the address of the shard which serves it and connect to it without intermediaries\. That is the fastest means of communication, but it prevents you from changing the number of shards or other details of your implementation without updating all the clients, which may be unachievable\. An intermediary may help\.

<ins>Pros</ins>: 

- Your system becomes isolated from its clients\.
- You can put generic aspects into the *Proxy* instead of implementing them in the shards\.
- *Proxies* are readily available\.


<ins>Cons</ins>: 

- The extra network hop increases latency unless you deploy the *Sharding Proxy* as an [*Ambassador*]({{< relref "../../extension-metapatterns/proxy.md#on-the-client-side-ambassador" >}}) \[[DDS]({{< relref "../../appendices/books-referenced.md#dds" >}})\] co\-located with every client, which brings back the issue of client software updates\.
- The *Sharding Proxy* is a single point of failure unless [*replicated*]({{< relref "../../basic-metapatterns/shards.md#persistent-copy-replica" >}})\.


## Move the integration logic into an Orchestrator

<figure>
<a href="/diagrams/Evolutions/Shards/Shards%20use%20Orchestrator.png">
<picture>
<source srcset="/diagrams/Evolutions/Shards/Shards%20use%20Orchestrator.svg" media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"/>
<source srcset="/diagrams/Evolutions/Shards/Shards%20use%20Orchestrator.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Shards/Shards%20use%20Orchestrator.png" alt="Shards use Orchestrator" loading="lazy" width="1045" height="244" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Shards]({{< relref "../../basic-metapatterns/shards.md" >}}), [Orchestrator]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: isolate the shards from awareness of each other\.

<ins>Prerequisite</ins>: the shards are coupled via their high\-level logic\.

When a high\-level scenario uses multiple shards \([*Scatter\-Gather* and *MapReduce*]({{< relref "../../extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) are the simplest examples\), the way to follow is to extract all such scenarios into a dedicated stateless module\. That makes the shards independent of each other\.

<ins>Pros</ins>: 

- The shards don’t have to be aware of each other\.
- The high\-level logic can be written in a high\-level language by a dedicated team\.
- The high\-level logic can be deployed independently\.
- The main code should become much simpler\.


<ins>Cons</ins>: 

- Latency will increase\.
- The *Orchestrator* becomes a single point of failure with a good chance to corrupt your data\.


<ins>Further steps</ins>:

- [*Shard* or *replicate* the *Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md#scaled" >}}) to support higher load and to remain online if it fails\.
- *Persist* the *Orchestrator* \(give it a dedicated database\) to make sure that it does not leave half\-committed transactions upon failure\.
- *Divide* the *Orchestrator* [into *Backends for Frontends*]({{< relref "../../appendices/evolutions/evolutions-of-layers-to-gain-flexibility.md#divide-the-orchestration-layer-into-backends-for-frontends" >}}) or a [*SOA*\-style layer]({{< relref "../../extension-metapatterns/orchestrator.md#a-service-per-use-case-soa-style" >}}) if you have multiple kinds of clients or workflows, correspondingly\.
