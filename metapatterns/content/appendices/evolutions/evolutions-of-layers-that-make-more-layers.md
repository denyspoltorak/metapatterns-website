+++
weight = 7
title = "Evolutions of Layers that make more layers"
description = "It is often possible to split the business logic layer in two, namely, integration or application logic (scenarios) and domain logic (business rules)."
images = ["/diagrams/Evolutions/Layers/Layers%20to%20Layers.svg"]
[sitemap]
  priority = 0.3
+++

# Evolutions of Layers that make more layers

Not all the layered architectures are equally layered\. A [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) with a [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) or database has already stepped into the realm of [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) but is far from reaping all of its benefits\. It may continue its journey in a few ways that [were earlier discussed]({{< relref "../../appendices/evolutions/evolutions-of-a-monolith-that-result-in-layers.md" >}}) for *Monolith*:

- Employing a *database* \(if you don’t use one\) lets you rely on a thoroughly optimized state\-of\-the\-art subsystem for data processing and storage\.
- [*Proxies*]({{< relref "../../extension-metapatterns/proxy.md" >}}) are similarly reusable generic modules to be added at will\.
- Implementing an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) on top of your system may improve programming experience and runtime performance for your clients\.


<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20to%20Layers.png">
<img src="/diagrams/Evolutions/Layers/Layers%20to%20Layers.svg" alt="Layers to Layers" loading="lazy" width="943" height="424" style="width:100%"/>
</a>
</figure>

It is also common to:

- Segregate the business logic into two [*layers*]({{< relref "../../basic-metapatterns/layers.md" >}})\.


## Split the business logic into two layers

<figure>
<a href="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.png">
<img src="/diagrams/Evolutions/Layers/Layers%20Split%20in%20Two.svg" alt="Layers Split in Two" loading="lazy" width="1027" height="243" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: let parts of the business logic vary in qualities, improve the structure of the code\.

<ins>Prerequisite</ins>: the high\-level and low\-level logic are loosely coupled\.

It is often possible to split a backend into integration \([orchestration]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}})\) and domain layers\. That allows for one team to specialize in customer use cases while the other one delves deep into the domain knowledge and infrastructure\.

<ins>Pros</ins>: 

- You get an extra development team\.
- High\-level use cases may be deployed separately from business rules\.
- The layers may diverge in technologies and styles\.
- The code may [become less complex]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md" >}})\.


<ins>Cons</ins>: 

- There is a small performance penalty\.
- In\-depth debugging becomes harder\.


<nav>

| \<\< [Evolutions of Shards that share logic]({{< relref "../../appendices/evolutions/evolutions-of-shards-that-share-logic.md" >}}) | ^ [Evolutions]({{< relref "../../appendices/evolutions/_index.md" >}}) ^ | [Evolutions of Layers that help large projects]({{< relref "../../appendices/evolutions/evolutions-of-layers-that-help-large-projects.md" >}}) \>\> |
| --- | --- | --- |

</nav>