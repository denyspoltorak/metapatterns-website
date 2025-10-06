+++
weight = 18
title = "Evolutions of a Combined Component"
description = "A stack of single-purpose layers can replace a Combined Component, buying you flexibility at the cost of development effort and, often, performance."
[sitemap]
  priority = 0.3
+++

# Evolutions of a Combined Component

The patterns that involve *orchestration* \([*API Gateway*]({{< relref "../../extension-metapatterns/combined-component.md#api-gateway" >}}), [*Event Mediator*]({{< relref "../../extension-metapatterns/combined-component.md#event-mediator" >}}), [*Enterprise Service Bus*]({{< relref "../../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\) may allow for [most of the evolutions]({{< relref "../../appendices/evolutions/evolutions-of-an-orchestrator.md" >}}) of the [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) metapattern by deploying multiple versions of the *Combined Component* that differ in their orchestration logic\. There is also a special evolution:

- Replace the *Combined Component* with several specialized ones


## Divide into specialized layers

<figure>
<a href="/diagrams/Evolutions/2/Multifunctional_%20Split.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Multifunctional_%20Split.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Multifunctional_%20Split.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Multifunctional_%20Split.png" alt="Multifunctional: Split" loading="lazy" width="1267" height="464" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: break out of *vendor lock\-in* \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\], gain flexibility\.

<ins>Prerequisite</ins>: you have lots of free time\.

If you feel that the *Combined Component* which your system relies on does not cover all your needs, or is too expensive, or unstable, then you may want to get rid of it by replacing it with generic single\-purpose tools or a homebrewed implementation that will always adapt to your circumstances\.

<ins>Pros</ins>:

- It’s free\.
- You’ll own your code\.
- Anything you write will fit your needs for as long as you spend time supporting it\.


<ins>Cons</ins>:

- Takes lots of work\.
- Performance may become worse because there will be more components on the requests’ path and also because the industry\-grade framework that you used could have been highly optimized\.
