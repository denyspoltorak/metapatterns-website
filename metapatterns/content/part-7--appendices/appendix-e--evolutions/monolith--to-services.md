+++
weight = 3
title = "Monolith: to Services"
+++

# Monolith: to Services

The final major drawback of [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}) is the cohesiveness of its code\. The rapid start of development begets a major obstacle for project growth: every developer needs to know the entire codebase to be productive and changes made by individual developers overlap and may break each other\. Such distress is usually solved by dividing the project into components along *subdomain boundaries* \(which tend to match [*bounded contexts*](https://martinfowler.com/bliki/BoundedContext.html) \[[DDD]({{< relref "../../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\)\. However, that requires a lot of work, and good boundaries and APIs are [hard to design](https://martinfowler.com/bliki/MonolithFirst.html)\. Thus many organizations prefer a slower iterative transition\.

- A *Monolith* can be split into [*Services*]({{< relref "../../part-2--basic-metapatterns/services.md" >}}) right away\.
- Or only the new features may be added as new services\.
- Or the weakly coupled parts of existing functionality may be separated, one at a time\.
- Some domains allow for sequential [data processing]({{< relref "../../part-1--foundations/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}) best described by [*Pipelines*]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}})\.


<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith_%20Services%20and%20Pipeline.png" alt="Monolith: Services and Pipeline" width=100%/>
</p>

</figure>

## Divide into Services

<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20to%20Services.png" alt="Monolith to Services" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Services]({{< relref "../../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: facilitate development by multiple teams, improve the code, decouple qualities of subdomains\.

<ins>Prerequisite</ins>: there is a natural way to split the business logic into loosely coupled subdomains, and the subdomain boundaries are sure to never change in the future\.

Splitting a *Monolith* into *Services* by subdomain [is risky in the early stages of a project](https://martinfowler.com/bliki/MonolithFirst.html) while the domain understanding is evolving \([in\-process *modules*]({{< relref "../../part-2--basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) are less risky but provide fewer benefits\)\. However, this is the way to go as soon as the codebase becomes unwieldy due to its size\.

<ins>Pros</ins>: 

- Supports multiple, relatively independent and specialized development teams\.
- Lowers the penalty imposed by the project’s size and complexity on the velocity of development and product quality\.
- Each team may choose the best fitting technologies for its service\.
- The services can differ in [non\-functional requirements](https://en.wikipedia.org/wiki/Non-functional_requirement)\.
- Flexible deployment and scaling\.
- A certain degree of error tolerance for asynchronous systems\.


<ins>Cons</ins>: 

- It takes lots of work to split a *Monolith*\.
- Any future changes to the overall structure of the domain will be hard to implement\.
- Sharing data between services is complicated and error\-prone\.
- System\-wide use cases are hard to understand and debug\.
- There is a moderate performance penalty for system\-wide use cases\.


## Add or split a service

<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20Split%20Service.png" alt="Monolith Split Service" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Services]({{< relref "../../part-2--basic-metapatterns/services.md" >}})\.

<ins>Goal</ins>: [stop digging](https://en.wikipedia.org/wiki/Law_of_holes), get some work for novices who don’t know the entire project\.

<ins>Prerequisite</ins>: the new functionality you are adding or the part you are splitting is weakly coupled to the bulk of the existing *Monolith*\.

If your [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}) is already hard to manage, but a new functionality is needed, you can try dedicating a separate service to the new feature\(s\)\. This way the *Monolith* does not become larger – it is even possible that you will move a part of its code to the newly established service\.

If you are not adding a new feature but need to change an old one – use the chance to make the existing *Monolith* smaller by first separating the functionality which you are going to change from its bulk\. At the very minimum this two\-step process lowers the probability of breaking something unrelated to the changes of behavior required\.

<ins>Pros</ins>: 

- The legacy code does not increase in size and complexity\.
- The new service is transferred to a dedicated team which does not need to know the legacy system\.
- The new service can be experimented with and even rewritten from scratch\.
- The likely faults of the new service don’t crash the main application\.
- The new service can be tested and deployed in isolation\.
- The new service can be scaled independently\.


<ins>Cons</ins>: 

- The new service will have a hard time sharing data or code with the main application\.
- Use cases that involve both the new service and the old application are hard to debug\.
- There is a moderate performance penalty for using the service\.


<ins>Further steps</ins>:

- [Continue disassembling](https://martinfowler.com/bliki/StranglerFigApplication.html) the *Monolith*\.


## Divide into a Pipeline

<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20to%20Pipeline.png" alt="Monolith to Pipeline" width=100%/>
</p>

</figure>

<ins>Patterns</ins>: [Pipeline]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}}) \([Services]({{< relref "../../part-2--basic-metapatterns/services.md" >}})\)\.

<ins>Goal</ins>: decrease the complexity of the code, make it easy to experiment with the steps of data processing, distribute the task over multiple CPU cores, processors or computers\.

<ins>Prerequisite</ins>: the domain can be represented as a sequence of coarse\-grained data processing steps\.

If you can treat your application as a chain of independent steps that transform the input data, you can rely on the OS to schedule them and you can also dedicate a development team to each of the steps\. This is the default solution for a system that [processes a stream]({{< relref "../../part-1--foundations/four-kinds-of-software.md#streaming-continuous-raw-data-input" >}}) of a single type of data \(video, audio, measurements\)\. It has excellent flexibility\.

<ins>Pros</ins>: 

- Nearly abolishes the influence of project size on development velocity\.
- The project’s teams become almost independent\.
- Flexible deployment and scaling\.
- Naturally supports event replay for reproducing bugs, testing or benchmarking individual components\.
- It is possible to have multiple implementations of each of the steps of data processing\.
- Does not need any manual scheduling or thread synchronization\.


<ins>Cons</ins>: 

- Latency may skyrocket\.
- As the number of supported scenarios grows, so does the number of components and *pipelines*\. Soon there’ll be nobody who understands the system as a whole\.


## Further steps

As your knowledge of the domain and your business requirements change, you may need to move some functionality between the services to keep them loosely coupled\. Sometimes you have to merge two or three services together\. So it goes\.

Systems of [*Services*]({{< relref "../../part-2--basic-metapatterns/services.md" >}}) or [*Pipelines*]({{< relref "../../part-2--basic-metapatterns/pipeline.md" >}}) are quite often extended with special kinds of [*layers*]({{< relref "../../part-2--basic-metapatterns/layers.md" >}}):

- [*Middleware*]({{< relref "../../part-3--extension-metapatterns/middleware.md" >}}) takes care of deployment, intercommunication and scaling of services\.
- [*Shared Repository*]({{< relref "../../part-3--extension-metapatterns/shared-repository.md" >}}) lets services operate on and communicate through [shared data]({{< relref "../../part-1--foundations/arranging-communication/shared-data.md" >}})\.
- [*Proxies*]({{< relref "../../part-3--extension-metapatterns/proxy.md" >}}) are ready\-to\-use components that add generic functionality to the system\.
- [*Orchestrator*]({{< relref "../../part-3--extension-metapatterns/orchestrator.md" >}}) encapsulates use cases that involve multiple services, so that the services don’t need to know about each other\.
- Finally, there are [*Combined Components*]({{< relref "../../part-3--extension-metapatterns/combined-component.md" >}}) that implement two or more of the above patterns in a single framework\. 


<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20to%20Services%20-%20Further%201.png" alt="Monolith to Services - Further 1" width=100%/>
</p>

</figure>

Each service, being a smaller *Monolith*, may evolve on its own\. Most of the evolutions of [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}) are applicable\. The most common examples include:

- [*Scaled \(Sharded\) Service*]({{< relref "../../part-2--basic-metapatterns/services.md#scaled-service" >}}) with a [*Load Balancer*]({{< relref "../../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) and [*Shared Database*]({{< relref "../../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) to support high load\.
- [*Layered Service*]({{< relref "../../part-2--basic-metapatterns/services.md#layered-service" >}}) to improve the code structure and decouple deployment of parts of a service\.
- [*Cell*]({{< relref "../../part-2--basic-metapatterns/services.md#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) \(*Service of Services*\) to involve multiple teams and technologies within a single subdomain\.
- [*Hexagonal Service*]({{< relref "../../part-2--basic-metapatterns/services.md#hexagonal-service" >}}) to escape vendor lock\-in\.


<figure>

<p align="center">
<img src="/Evolutions/Monolith/Monolith%20to%20Services%20-%20Further%202.png" alt="Monolith to Services - Further 2" width=100%/>
</p>

</figure>

<nav>

| \<\< [Monolith: to Layers]({{< relref "../../part-7--appendices/appendix-e--evolutions/monolith--to-layers.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Monolith: to Plugins]({{< relref "../../part-7--appendices/appendix-e--evolutions/monolith--to-plugins.md" >}}) \>\> |
| --- | --- | --- |

</nav>



