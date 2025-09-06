+++
weight = 16
title = "Proxy:"
+++

# Proxy:

It usually makes little sense to get rid of a [*Proxy*]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}) once it is integrated into a system\. Its only real drawback is a slight increase in latency for user requests which may be helped through creation of [bypass channels]({{< relref "../../part-3--extension-metapatterns/proxy.md#half-proxy" >}}) between the clients and a service that needs low latency\. The other drawback of the pattern, the *Proxy*’s being a single point of failure, is countered by deploying multiple instances of the *Proxy*\.

As *Proxies* are usually third\-party products, there is very little we can change about them:

- We can add another kind of a *Proxy* on top of the existing one\.
- We can use a stack of *Proxies* per client, making [*Backends for Frontends*]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.


## Add another Proxy

<figure>

<p align="center">
<img src="/Evolutions/2/Proxy add Proxy.png" alt="Proxy add Proxy" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Proxy]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}), [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: avoid implementing generic functionality\.

<ins>Prerequisite</ins>: you don't have this kind of *Proxy* yet\.

A system is not limited to a single kind of *Proxies*\. As a *Proxy* represents your system without changing its function, *Proxies* are transparent, thus they are stackable\.

It often makes sense to colocate software *Proxies* or use a multifunctional *Proxy* to reduce the number of network hops between the clients and the system\. However, in a highly loaded system *Proxies* may be resource\-hungry, thus in some cases colocation strikes back\.

<ins>Pros</ins>: 

- You get another aspect of your system implemented for you\.


<ins>Cons</ins>: 

- Latency degrades\.
- More work for admins\.
- Another point of possible failure\.


## Deploy a Proxy per client type

<figure>

<p align="center">
<img src="/Evolutions/2/Proxy to Backends for Frontends.png" alt="Proxy to Backends for Frontends" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Proxy]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}), [Backends for Frontends]({{< relref "../../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

<ins>Goal</ins>: let the aspects of communication vary among kinds of clients\.

<ins>Prerequisite</ins>: your system serves several kinds of clients\.

If you have internal and external clients, or admins and users, you may want to vary the setup of *Proxies* for each kind of client, sometimes to the extent of physically separating network communication paths, so that each kind of client is treated according to its bandwidth, priority and permissions\.

<ins>Pros</ins>: 

- It is easy to set up various aspects of communication for a group of clients\.


<ins>Cons</ins>: 

- More work for admins as the *Proxies* are duplicated\.


<nav>

| \<\< [Shared Repository:]({{< relref "../../part-7--appendices/appendix-e--evolutions/shared-repository-.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Orchestrator:]({{< relref "../../part-7--appendices/appendix-e--evolutions/orchestrator-.md" >}}) \>\> |
| --- | --- | --- |

</nav>



