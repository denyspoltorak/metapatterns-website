+++
weight = 5
title = "Comparison of communication styles"
description = "Orchestration helps with complex use cases, shared data supports data-centric domains, while choreography is highly scalable."
images = ["/diagrams/Web/og/Favicon-plain.png"]
[sitemap]
  priority = 0.5
+++

# Comparison of communication styles {anchor=false}

We have briefly discussed three approaches to communication: orchestration, choreography, and shared data\. Let’s recall when it makes sense to use each of them\.

[*Orchestration*]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) is built around use cases\. They are easy to program and add, no matter how complex they become\. Thus, if your \(sub\)domain is coupled, or your understanding of it is still evolving, this is the way to go, as you will be able to change the high\-level logic in any imaginable way because you express it as convenient imperative code\.

[*Shared data*]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}}) is all about… er… domain data\. If you really \(believe that you\) know your domain, and it deals with coupled data – this is your chance\. You may even add in an *Orchestrator* if there are use cases that involve multiple subdomains\. The business logic is going to be easy to extend while changes to the data schema are sure to cause havoc\.

[*Choreography*]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md" >}}) pays off for weakly coupled domains with a few simple use cases\. It has good performance and flexibility, but lacks the expressive power of orchestration and becomes very messy as the number of tasks and components grows\. It works best with independent teams and delayed processing – when users do not wait for the immediate results of their actions\.

There is advice [from Microsoft](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography) and \[[DEDS]({{< relref "../../appendices/books-referenced.md#deds" >}})\] which makes perfect sense: use choreography for communication between *bounded contexts* \(subdomains\) but revert to orchestration \(or maybe shared data\) inside each context\. Indeed, subdomains are likely to be loosely coupled while most user requests don’t traverse subdomain boundaries – which kindles hope that their interactions are few and not time\-critical\. If we follow the advice, we get [*Cell\-Based Architecture*]({{< relref "../../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \([WSO2 definition](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)\), which collects the best of two worlds: orchestration and/or shared data for strongly coupled parts and choreography between them\.

<figure>
<a href="/diagrams/Communication/Cell-Based%20Architecture.png">
<picture>
<source srcset="/diagrams/Communication/Cell-Based%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Cell-Based%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Cell-Based%20Architecture.png" alt="Cell-Based Architecture" loading="lazy" width="923" height="374" style="width:100%"/>
</picture>
</a>
</figure>

By the way, you could have noticed a few odd cases:

- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) in a [control system]({{< relref "../../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) does not run scenarios and its mode of action resembles choreography\.
- A choreographed system may use a [shared message format]({{< relref "../../extension-metapatterns/shared-repository.md#inexact-stamp-coupling" >}}), which makes it resemble a system with shared data, even though no shared database is present\.
- A shared database may be used to [implement messaging]({{< relref "../../extension-metapatterns/shared-repository.md#persistent-event-log-shared-event-store" >}}) for an orchestrated or choreographed system, effectively becoming a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}})\.


That likely means that our distinction between the modes of communication is a bit artificial and there may be a yet unknown deeper model to look for\.