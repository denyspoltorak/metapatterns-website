+++
weight = 14
title = "Evolutions of a Middleware"
description = "It is possible to add a second specialized Middleware or integrate several systems through a hierarchy of Middlewares."
[sitemap]
  priority = 0.3
+++

# Evolutions of a Middleware {anchor=false}

A [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) is unlikely to be removed \(though it may be replaced\) once it is built into a system\. There are few evolutions as a *Middleware* is a third\-party product and is unlikely to be messed with:

- If the *Middleware* in use does not fit the preferred mode of communication between some of your services, there is an option to deploy a second specialized *Middleware*\.
- If several existing systems need to be merged, that is accomplished by adding yet another layer of *Middleware*, resulting in a [*Bottom\-Up Hierarchy \(Bus of Buses\)*]({{< relref "../../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}})\.


## Add a secondary Middleware

<figure>
<a href="/diagrams/Evolutions/2/Middleware%20add%20Middleware.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Middleware%20add%20Middleware.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Middleware%20add%20Middleware.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Middleware%20add%20Middleware.png" alt="Middleware add Middleware" loading="lazy" width="1223" height="344" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Middleware]({{< relref "../../extension-metapatterns/middleware.md" >}})\.

<ins>Goal</ins>: support specialized communication between [scaled]({{< relref "../../basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) services\.

<ins>Prerequisite</ins>: the system relies on a *Middleware* for scaling\.

If the current *Middleware* is too generic for the system’s needs, you can add another one for specialized communication\. The new *Middleware* does not manage the instances of the services\.

<ins>Pros</ins>: 

- Supports specialized communication with no need to write code for tracking the instances of services\.


<ins>Cons</ins>: 

- You still need to notify the new *Middleware* when an instance of a service is created or dies\.
- There is an extra component to administer\.


## Merge two systems by building a Bottom\-Up Hierarchy

<figure>
<a href="/diagrams/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Middleware%20to%20Bus%20of%20Buses.png" alt="Middleware to Bus of Buses" loading="lazy" width="1903" height="405" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Bottom\-up Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md#bottom-up-hierarchy-bus-of-buses-network-of-networks" >}}) \([Hierarchy]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}), [Middleware]({{< relref "../../extension-metapatterns/middleware.md" >}})\)\.

<ins>Goal</ins>: integrate two systems without a heavy refactoring\.

<ins>Prerequisite</ins>: both systems use *Middleware*s\.

If we cannot change the way each subsystem’s services use its *Middleware*, we should add a new *Middleware* to connect the existing *Middleware*s\.

<ins>Pros</ins>: 

- No need to touch anything in the existing services\.


<ins>Cons</ins>: 

- Performance suffers from the double conversion between protocols\.
- There is a new component to fail \(miserably\)\.
