+++
weight = 11
title = "Evolutions of Services that add or remove services"
description = "A new service can be added, an existing one can be split in two, or multiple services can be merged if they become tightly coupled."
[sitemap]
  priority = 0.3
+++

# Evolutions of Services that add or remove services

[*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) work well when each service matches a subdomain and is developed by a single team\. If those premises change, you’ll need to restructure the system:

- A new feature request may emerge outside of any of the existing subdomains, creating a new service\.
- A service may grow too large to be developed by a single team, calling for division\.
- Two services may become so strongly coupled that they fare better when merged together\.
- The entire system may need to be glued back into a [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) if domain knowledge changes or interservice communication strongly degrades performance\.


## Add or split a service

<figure>
<a href="/diagrams/Evolutions/Services/Services_%20Split.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services_%20Split.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services_%20Split.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services_%20Split.png" alt="Services: Split" loading="lazy" width="1303" height="251" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Services]({{< relref "../../basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: get one more team to work on the project, decrease the size of an existing service\.

<ins>Prerequisite</ins>: there is a loosely coupled \(new or existing\) subdomain that does not have a dedicated service \(yet\)\.

If you need to add a new functionality that does not naturally fit into one of the existing services, you may create a new service and maybe get a new team for it\.

If one of your services has grown too large, you should look for a way to subdivide it \(likely through a [*Cell*]({{< relref "../../basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) stage with a shared [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) and [*database*]({{< relref "../../extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\) to decrease the size and, correspondingly, complexity of its code and get multiple teams to work on the resulting \(sub\)services\. However, that makes sense only if the old service is not highly cohesive – otherwise [the resulting subsystem may be more complex]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md#coupling-and-cohesion" >}}) than the original service\.

<ins>Pros</ins>: 

- You get an extra development team\.
- The complexity of the code decreases \(splitting an existing service\) or does not increase \(adding a new one\)\.
- The new service is independently scalable\.


<ins>Cons</ins>: 

- You add to the operations complexity by creating a new system component and several inter\-component dependencies\.
- There is a new point of failure, which means that bugs and outages become more likely\.
- Performance \(or at least latency and cost efficiency\) of the system will deteriorate because interservice communication is slow\.
- You may have a hard time debugging use cases that involve both the old and new service\.


## Merge services

<figure>
<a href="/diagrams/Evolutions/Services/Services_%20Merge.png">
<picture>
<source srcset="/diagrams/Evolutions/Services/Services_%20Merge.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Services/Services_%20Merge.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Services/Services_%20Merge.png" alt="Services: Merge" loading="lazy" width="1083" height="245" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Services]({{< relref "../../basic-metapatterns/services.md" >}}), [Monolith]({{< relref "../../basic-metapatterns/monolith.md" >}}) or [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: accept the coupling of subdomains and improve performance\.

<ins>Prerequisite</ins>: the services use compatible technologies\.

If you see that several services communicate with each other almost as intensely as they call their internal methods, they probably belong together\.

If your use cases have too high a latency or you pay too much for CPU and traffic, the issue may originate with the interservice communication and merging the services should help\. No services, no pain\.

Alternatively, as domain knowledge changes \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\], you may have to merge much of the code together only to subdivide it later along updated subdomain boundaries\. Which means you face [lots of work for no reason](https://martinfowler.com/bliki/MonolithFirst.html)\.

<ins>Pros</ins>: 

- Improved performance\.
- It becomes easy for parts of the merged code to access each other and share data\.
- The new merged *service* or *Monolith* is easier to debug than the original *Services*\.


<ins>Cons</ins>: 

- The development teams become even more interdependent\.
- There is no good way to vary qualities by subdomain\.
- You lose granular scalability by subdomain\.
- The merged codebase may be too large for comfortable development\.
- If anything fails, everything fails\.
