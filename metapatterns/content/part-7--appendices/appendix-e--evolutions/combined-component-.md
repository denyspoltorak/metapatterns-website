+++
weight = 18
title = "Combined Component:"
description = A stack of single-purpose layers can replace a Combined Component, buying you flexibility at the cost of development effort and, often, performance.
+++

# Combined Component:

The patterns that involve *orchestration* \([*API Gateway*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}), [*Event Mediator*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}), [*Enterprise Service Bus*]({{< relref "../../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}})\) may allow for [most of the evolutions]({{< relref "../../part-7--appendices/appendix-e--evolutions/orchestrator-.md" >}}) of the [*Orchestrator*]({{< relref "../../part-3--extension-metapatterns/orchestrator.md" >}}) metapattern by deploying multiple versions of the *Combined Component* that differ in their orchestration logic\. There is also a special evolution:

- Replace the *Combined Component* with several specialized ones


## Divide into specialized layers

<figure style="text-align:center">
<a href="/Evolutions/2/Multifunctional_%20Split.png" style="outline:none">
<img src="/Evolutions/2/Multifunctional_%20Split.png" alt="Multifunctional: Split" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../part-2--basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: break out of *vendor lock\-in* \[[DDD]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\], gain flexibility\.

<ins>Prerequisite</ins>: you have lots of free time\.

If you feel that the *Combined Component* which your system relies on does not cover all your needs, or is too expensive, or unstable, then you may want to get rid of it by replacing it with generic single\-purpose tools or a homebrewed implementation that will always adapt to your circumstances\.

<ins>Pros</ins>:

- It’s free\.
- You’ll own your code\.
- Anything you write will fit your needs for as long as you spend time supporting it\.


<ins>Cons</ins>:

- Takes lots of work\.
- Performance may become worse because there will be more components on the requests’ path and also because the industry\-grade framework that you used could have been highly optimized\.


<nav>

| \<\< [Orchestrator:]({{< relref "../../part-7--appendices/appendix-e--evolutions/orchestrator-.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Appendix F\. Format of a metapattern\.]({{< relref "../../part-7--appendices/appendix-f--format-of-a-metapattern.md" >}}) \>\> |
| --- | --- | --- |

</nav>



