+++
weight = 7
title = "Layers: make more layers"
+++

# Layers: make more layers

Not all the layered architectures are equally layered\. A [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}) with a [*Proxy*]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}) or database has already stepped into the realm of [*Layers*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}) but is far from reaping all of its benefits\. It may continue its journey in a few ways that [were earlier discussed]({{< relref "../../part-7--appendices/appendix-e--evolutions/monolith--to-layers.md" >}}) for *Monolith*:

- Employing a *database* \(if you don’t use one\) lets you rely on a thoroughly optimized state\-of\-the\-art subsystem for data processing and storage\.
- [*Proxies*]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}) are similarly reusable generic modules to be added at will\.
- Implementing an [*Orchestrator*]({{< relref "../../part-3--extension-metapatterns/orchestrator.md" >}}) on top of your system may improve programming experience and runtime performance for your clients\.


<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers%20to%20Layers.png" style="outline:none">
<img src="/Evolutions/Layers/Layers%20to%20Layers.png" alt="Layers to Layers" style="width:100%"/>
</a>
</figure>

It is also common to:

- Segregate the business logic into two [*layers*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}})\.


## Split the business logic into two layers

<figure style="text-align:center">
<a href="/Evolutions/Layers/Layers%20Split%20in%20Two.png" style="outline:none">
<img src="/Evolutions/Layers/Layers%20Split%20in%20Two.png" alt="Layers Split in Two" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: let parts of the business logic vary in qualities, improve the structure of the code\.

<ins>Prerequisite</ins>: the high\-level and low\-level logic are loosely coupled\.

It is often possible to split a backend into integration \([orchestration]({{< relref "../../part-1--foundations/arranging-communication/orchestration.md" >}})\) and domain layers\. That allows for one team to specialize in customer use cases while the other one delves deep into the domain knowledge and infrastructure\.

<ins>Pros</ins>: 

- You get an extra development team\.
- High\-level use cases may be deployed separately from business rules\.
- The layers may diverge in technologies and styles\.
- The code may [become less complex]({{< relref "../../part-1--foundations/modules-and-complexity.md" >}})\.


<ins>Cons</ins>: 

- There is a small performance penalty\.
- In\-depth debugging becomes harder\.


<nav>

| \<\< [Shards: share logic]({{< relref "../../part-7--appendices/appendix-e--evolutions/shards--share-logic.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Layers: help large projects]({{< relref "../../part-7--appendices/appendix-e--evolutions/layers--help-large-projects.md" >}}) \>\> |
| --- | --- | --- |

</nav>



