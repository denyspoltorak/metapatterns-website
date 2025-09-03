+++
weight = 14
title = "Middleware:"
+++

# Middleware:

A [*Middleware*]({{< relref "../../part-3--extension-metapatterns/middleware.md" >}}) is unlikely to be removed \(though it may be replaced\) once it is built into a system\. There are few evolutions as a *Middleware* is a third\-party product and is unlikely to be messed with:

- If the *Middleware* in use does not fit the preferred mode of communication between some of your services, there is an option to deploy a second specialized *Middleware*\.
- If several existing systems need to be merged, that is accomplished by adding yet another layer of *Middleware*, resulting in a [*Bottom\-Up Hierarchy \(Bus of Buses\)*]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}})\.


## Add a secondary middleware

<p align="center">
<img src="/Evolutions/2/Middleware add Middleware.png" alt="Middleware add Middleware" width=100%/>
</p>

<ins>Patterns</ins>: [Middleware]({{< relref "../../part-3--extension-metapatterns/middleware.md" >}})\.

<ins>Goal</ins>: support specialized communication between [scaled]({{< relref "../../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) services\.

<ins>Prerequisite</ins>: the system relies on a *Middleware* for scaling\.

If the current *Middleware* is too generic for the system’s needs, you can add another one for specialized communication\. The new *Middleware* does not manage the instances of the services\.

<ins>Pros</ins>: 

- Supports specialized communication with no need to write code for tracking the instances of services\.


<ins>Cons</ins>: 

- You still need to notify the new *Middleware* when an instance of a service is created or dies\.
- There is an extra component to administer\.


## Merge two systems by building a Bottom\-Up Hierarchy

<p align="center">
<img src="/Evolutions/2/Middleware to Bus of Buses.png" alt="Middleware to Bus of Buses" width=100%/>
</p>

<ins>Patterns</ins>: [Bottom\-up Hierarchy]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}}) \([Hierarchy]({{< relref "../../part-4--fragmented-metapatterns/hierarchy.md" >}}), [Middleware]({{< relref "../../part-3--extension-metapatterns/middleware.md" >}})\)\.

<ins>Goal</ins>: integrate two systems without a heavy refactoring\.

<ins>Prerequisite</ins>: both systems use *Middleware*s\.

If we cannot change the way each subsystem’s services use its *Middleware*, we should add a new *Middleware* to connect the existing *Middleware*s\.

<ins>Pros</ins>: 

- No need to touch anything in the existing services\.


<ins>Cons</ins>: 

- Performance suffers from the double conversion between protocols\.
- There is a new component to fail \(miserably\)\.


<nav>

| \<\< [Pipeline:]({{< relref "../../part-7--appendices/appendix-e--evolutions/pipeline-.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Shared Repository:]({{< relref "../../part-7--appendices/appendix-e--evolutions/shared-repository-.md" >}}) \>\> |
| --- | --- | --- |

</nav>



