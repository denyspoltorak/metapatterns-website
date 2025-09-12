+++
weight = 9
title = "Services"
description = Services are components dedicated to subdomains. They may vary in size, internal structure and technologies. A service can be further subdivided into a Cell.
+++

# Services

<figure style="text-align:center">
<a href="/Main/Services.png" style="outline:none">
<img src="/Main/Services.png" alt="Services" style="width:100%"/>
</a>
</figure>

*Divide and conquer\.* Gain flexibility through decoupling subdomains\.

<ins>Known as:</ins> Services, Domain Services \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}}) and [SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}), but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#domain-services" >}}) [DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\.

<ins>Variants:</ins> [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) has a dedicated chapter\. Many modifications are listed in the [Evolutions section]({{< relref "#evolutions" >}})\.

By isolation:

- Synchronous modules: [Modular Monolith](https://medium.com/design-microservices-architecture-with-patterns/microservices-killer-modular-monolithic-architecture-ac83814f6862) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] \(Modulith\),
- Asynchronous modules: [Modular Monolith](https://medium.com/design-microservices-architecture-with-patterns/microservices-killer-modular-monolithic-architecture-ac83814f6862) \(Modulith\) / [Embedded Actors](https://medium.com/itnext/introduction-to-software-architecture-with-actors-part-1-89de6000e0d3),
- Multiple processes,
- Distributed runtime: [Function as a Service](https://www.bmc.com/blogs/serverless-faas/) \(FaaS\) \(including [Nanoservices](https://medium.com/@ido.vapner/unlocking-the-power-of-nano-services-a-new-era-in-microservices-architecture-22647ea36f22)\) / [Backend Actors](https://volodymyrpavlyshyn.medium.com/actors-actor-systems-as-massively-distributed-scalability-architecture-5e40f5ea9e86),
- Distributed services: Service\-Based Architecture \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] / Space\-Based Architecture \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] / Microservices\.


By communication:

- Direct method calls,
- [RPC](https://en.wikipedia.org/wiki/Remote_procedure_call)s and commands \(request/confirm pairs\),
- Notifications \(pub/sub\) and shared data,
- \(inexact\) No communication\.


By size:

- Whole subdomain: Domain Services \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\],
- Part of a subdomain: Microservices,
- Class\-like: [Actors](https://volodymyrpavlyshyn.medium.com/actors-actor-systems-as-massively-distributed-scalability-architecture-5e40f5ea9e86),
- Single function: [FaaS](https://en.wikipedia.org/wiki/Function_as_a_service) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] / [Nanoservices](https://medium.com/@ido.vapner/unlocking-the-power-of-nano-services-a-new-era-in-microservices-architecture-22647ea36f22)\.


By internal structure:

- Monolithic service,
- Layered service,
- Hexagonal service,
- Scaled service,
- Cell \([WSO2 definition](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)\) \(service of services\) / Domain \([Uber definition](https://www.uber.com/blog/microservice-architecture/)\) / Cluster \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\.


Examples:

- Service\-Based Architecture \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}}) but [not]({{< relref "../part-6--analytics/ambiguous-patterns.md#service-based-architecture" >}}) [DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\],
- Microservices \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\],
- [Actors](https://volodymyrpavlyshyn.medium.com/actors-actor-systems-as-massively-distributed-scalability-architecture-5e40f5ea9e86),
- \(inexact\) Nanoservices \(API layer\),
- \(inexact\) Device Drivers\.


<ins>Structure:</ins> A component per subdomain\.

<ins>Type:</ins> Main\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Supports large codebases | Global use cases are hard to debug |
| Multiple development teams and technologies | Poor latency in global use cases |
| Forces may vary by subdomain | No good way to share state between services |
|  | The domain structure should never change |
|  | Operational complexity |


<ins>References:</ins> \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] has a chapter on *Service\-Based Architecture*; \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] is dedicated to *Microservices*\.

Splitting a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) by *subdomain* allows for mostly independent properties, development, and deployment of the resulting components\. However, for the system to benefit from the division, the subdomains must be loosely coupled and, ideally, of comparable size\. In that case the partitioning can reduce complexity of the project’s code by cutting accidental dependencies between the subdomains\. Moreover, if one of the resulting services grows unmanageably large, it can often be further partitioned by sub\-subdomains to form a [*Cell*]({{< relref "#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\. This flexibility is paid for through the complexity and performance of use cases which involve multiple subdomains\. Another issue to remember is that boundaries between services are [nearly impossible](https://martinfowler.com/bliki/MonolithFirst.html) to move at later project stages as the services grow to vary in technologies and implementation styles, thus separation into services assumes perfect practical knowledge of the domain and relatively stable requirements\.

<aside>

> [Research](https://www.qsm.com/team-size-can-be-key-successful-software-project) shows that when more than five programmers work on the same subject, their performance degrades\. Therefore, if we want our employees to be efficient, they should be grouped into small teams and each team should be given ownership of a dedicated component\.

</aside>

### Performance

Interservice communication is relatively slow and resource\-consuming, therefore it should be kept to a minimum\.

<figure style="text-align:center">
<a href="/Performance/Services.png" style="outline:none">
<img src="/Performance/Services.png" alt="Services" style="width:100%"/>
</a>
</figure>

The perfect case is when a single service has enough authority to answer a client’s request or process an event\. That case should not be that rare as a service covers a whole subdomain while subdomains are expected to be loosely coupled \(by definition\)\.

Worse is when an event starts a chain reaction throughout the system, likely looping back a response to the original service or changing the target state of another controlled subsystem\.

In the slowest scenario a service needs to synchronize its state with multiple other services, usually via *locks* and *distributed transactions*\.

Multiple [instances]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of an individual service may be deployed to improve throughput of the system\. However, such a case will likely need a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) or [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) to distribute interservice requests among the instances and a [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) to store and synchronize any non\-shardable \(accessed by several instances\) state\.

### Dependencies

When we see a service to *request* help from other services and then receive the results \(in a *confirmation* message\), that service [*orchestrates*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) the services it uses\. Services often orchestrate each other because the subdomain a service is dedicated to is not independent of other subdomains\.

<figure style="text-align:center">
<a href="/Dependencies/Services-1.png" style="outline:none">
<img src="/Dependencies/Services-1.png" alt="Services-1" style="width:100%"/>
</a>
</figure>

Another way for services to communicate is [*choreography*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) – when a service sends a *command* or publishes a *notification* and does not expect any response\. This is characteristic of [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) which are covered in the next chapter\. Right now we should note that orchestration and choreography may be intermixed, in which case a service depends on all the services it uses or subscribes to\.

<figure style="text-align:center">
<a href="/Dependencies/Services-2.png" style="outline:none">
<img src="/Dependencies/Services-2.png" alt="Services-2" style="width:100%"/>
</a>
</figure>

If the system relies on notifications \(services publish *domain events*\), it is possible to avoid interservice *queries* \(pairs of a *read* request and confirmation with the data retrieved\) by aggregating data from notifications in a [*CQRS* \(or *materialized*\) *View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\], which can reside [in memory](https://martinfowler.com/bliki/MemoryImage.html) or in a database\. Views can be planted inside every service that needs data owned by other services or can be gathered into a dedicated [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\. Though the main goal of *CQRS Views* is to resolve distributed joins from databases of multiple services, they also help [remove dependencies]({{< relref "../part-6--analytics/comparison-of-architectural-patterns/indirection-in-commands-and-queries.md#query-olap-systems" >}}) in the code of services and optimize out interservice queries, simplifying APIs and improving performance\. Further examples will be discussed in the chapter on [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.

<figure style="text-align:center">
<a href="/Dependencies/Services-3.png" style="outline:none">
<img src="/Dependencies/Services-3.png" alt="Services-3" style="width:100%"/>
</a>
</figure>

In general, a large service should wrap its dependencies with an [*Anticorruption Layer*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\], following the ideas of [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}})\. The layer consists of *Adapters* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#gof" >}})\] between the internal domain model of the service and the APIs of the components it uses\. The *Adapters* isolate the business logic from the external environment, granting that no change in the interface of an external service or library may ever take much work to support on the side of the team that writes our business logic as all the ensuing updates are limited to a small adapter\.

<figure style="text-align:center">
<a href="/Dependencies/Services-4.png" style="outline:none">
<img src="/Dependencies/Services-4.png" alt="Services-4" style="width:100%"/>
</a>
</figure>

### Applicability

*Services* are <ins>good</ins> for:

- *Large projects\.* With multiple services developed independently, a project may grow well above 1 000 000 lines of code and still be comfortable to work on as every team needs to know only the medium\-sized component it owns\.
- *Specialized teams\.* Each service would often be written and supported by a dedicated team that invests its time in learning its subdomain\. This way no one needs to have a detailed knowledge of the full set of requirements, which is next to impossible in large domains\.
- *Varied forces*\. In system and embedded programming, components of wildly varying behaviors need to be managed\. Each of them is controlled by a dedicated service \(called *driver*\) which adapts to the specifics of the managed subsystem\.
- *Flexible scaling\.* Some services may be under more load than others\. It makes sense to deploy multiple instances of heavily loaded services\.


*Services* <ins>should be avoided</ins> in:

- *Cohesive domains\.* If everything strongly depends on everything, any attempt to cut the knot with interfaces is going to [make things worse]({{< relref "../part-1--foundations-of-software-architecture/modules-and-complexity.md#coupling-and-cohesion" >}}) unless the project is already dying because of its huge codebase, in which case you have nothing to lose\.
- *Unfamiliar domains*\. If you don’t understand the intricacies of the system you are going to build, you may [misalign the interfaces](https://martinfowler.com/bliki/MonolithFirst.html) and, by the time that the mistakes come to light, the architecture will be too hard to change \[[LDDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#lddd" >}})\]\. The coupled *Services* you get may actually be worse than a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}})\.
- *Quick start*\. It takes effort to design good interfaces and contracts for *Services* and managing multiple deployment units is not free of trouble\. Debugging will be an issue\.
- *Low latency*\. If the system as a whole needs to react to events in real time, complex services should be avoided\. Nevertheless, an individual service can provide low latency for local use cases \(when a single service has enough authority to react to the incoming event\), wherefore [simple non\-blocking actors]({{< relref "#asynchronous-modules-modular-monolith-modulith-embedded-actors" >}}) are widely used in [control software]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\.


### Relations

<figure style="text-align:center">
<a href="/Relations/Services.png" style="outline:none">
<img src="/Relations/Services.png" alt="Services" style="width:100%"/>
</a>
</figure>

- Division by subdomain can be applied to [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) to form [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) \(layers of services\); to [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), or [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) to make [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}); to a [\(shared\) database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) resulting in [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\.
- Services can be extended with a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}), or [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.
- Each service can be implemented by [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) \(making a [*layered service*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}})\), [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}), or a [*Cell*]({{< relref "#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}})\.


## Variants by isolation

Division by subdomain is so commonplace and varied that no universal terminology emerged over the years\. Below is my summary, in no way complete, of several ways such systems vary\. Each section lists the well\-known architectures it applies to\.

First and foremost, there are multiple grades between a cohesive [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) and distributed *Services*\. You should choose incrementally when to stop because the benefits of these next stages \(color\-coded below\) may not outweigh their drawbacks for your project\.

<aside>

> I review here only the most common options while a few more esoteric architectures are found in [Volodymyr Pavlyshyn’s overview](https://volodymyrpavlyshyn.medium.com/monoliths-microlith-moduliths-self-contained-systems-a-system-of-systems-nano-services-cf3e9e1869c0)\. 

</aside>

### Synchronous modules: Modular Monolith \(Modulith\)

The first stage to take when designing a large project is the division of the codebase into loosely coupled modules that match subdomains \(*bounded contexts* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\)\. If successful, that parallelizes development to a team per module while the entire application still runs in a single process, thus it stays easy to debug, the modules can share data, and any crash kills the whole system \(you don’t need to take care of partial failures\)\. You pay by establishing boundaries which will not be easy to move in the future\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| <span style="color:green">Multi\-team development</span> | <span style="color:red">Subdomain boundaries are settled</span> |


### Asynchronous modules: Modular Monolith \(Modulith\), Embedded Actors

The next stage is separating the modules’ execution threads and data\. Each module becomes a kind of [*actor*](https://en.wikipedia.org/wiki/Actor_model) that communicates with other components through messaging\. Now your modules don’t block each other’s execution and you can [replay events](http://ithare.com/chapter-vc-modular-architecture-client-side-on-debugging-distributed-systems-deterministic-logic-and-finite-state-machines/) at the cost of nightmarish debugging and no clean way to share data between or synchronize the state of the components\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Multi\-team development | Subdomain boundaries are settled |
| <span style="color:green">Event replay</span> | <span style="color:red">No good way to share data or synchronize state</span> |
| <span style="color:green">Some independence of module qualities</span> | <span style="color:red">Hard to debug</span> |


### Multiple processes

There is also the option of running system components as separate binaries which lets them vary in technologies, allows for granular updates, and addresses stability \(a web browser does not stop when one of its tabs crashes\)\. But it adds a whole dimension of error recovery and partially executed scenarios\. Moreover, divergency of technologies makes moving pieces of code between the services impossible\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Multi\-team development | Subdomain boundaries are <span style="color:red">frozen</span> |
| Event replay | No good way to share data or synchronize state |
| Independence of component qualities <span style="color:green">and technologies</span> | Hard to debug |
| <span style="color:green">Single\-component updates</span> | <span style="color:red">Needs error recovery routines</span> |
| <span style="color:green">Software fault isolation</span> | <span style="color:red">Data inconsistencies after partial crashes</span> |
| <span style="color:green">Limited granular scalability</span> |  |


### Distributed runtime: Function as a Service \(FaaS\) \(including Nanoservices\), Backend Actors

Modern distributed [runtimes](https://en.wikipedia.org/wiki/Runtime_system) create a virtual namespace that may be deployed on a single machine or over a network\. They may redistribute running components over servers in a way to minimize network communication and may offer distributed debugging\. With [*Actors*](https://en.wikipedia.org/wiki/Actor_model), if one of them crashes, that generates a message to another actor which may decide on how to handle the error\. The convenience of using a runtime has the dark side of vendor lock\-in\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Multi\-team development | Subdomain boundaries are frozen |
| Event replay | No good way to share data or synchronize state |
| Independence of component qualities ~and technologies~ | Hard to debug |
| Single\-component updates | Needs error recovery routines |
| <span style="color:green">Full</span> fault isolation | ~Data inconsistencies after partial crashes~ |
| <span style="color:green">Full dynamic</span> granular scalability | <span style="color:red">Vendor lock\-in</span> |
|  | <span style="color:red">Moderate communication overhead</span> |
|  | <span style="color:red">Moderate performance overhead caused by the framework</span> |


### Distributed services: Service\-Based Architecture, Space\-Based Architecture, Microservices

Fully autonomous services run on dedicated servers or virtual machines\. This way you employ resources of multiple servers, but the communication between them is both unstable \(requests may be lost, reordered or duplicated\) and slow and debugging tends to be very hard\. [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\-based \([*Microservices*]({{< relref "../part-5--implementation-metapatterns/mesh.md#service-mesh" >}}) and [*Space\-Based*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\) architectures provide dynamic scaling under load\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Multi\-team development | Subdomain boundaries are frozen |
| Event replay | No ~<span style="color:red">good</span>~ way to share data or synchronize state |
| Independence of component qualities and technologies | <span style="color:red">Very</span> hard to debug |
| Single\-component updates | Needs error recovery routines |
| <span style="color:green">Full</span> fault isolation | Data inconsistencies after partial crashes |
| <span style="color:green">Full \(dynamic for *Mesh*\)</span> granular scalability | <span style="color:red">High communication overhead</span> |


## Variants by communication

Services also differ in the way they communicate which influences some of their properties:

### Direct method calls

When components run inside the same process and share execution threads, one component can call another\. That is blazingly fast and efficient, but you should take care to protect the module’s state from simultaneous access by multiple threads \(and yes, [*deadlocks*](https://en.wikipedia.org/wiki/Deadlock_(computer_science)) do happen in practice\)\. Moreover, it is hard to know what the module you call is going to call in its turn, while you are waiting on it – thus no matter how much you optimize your code, its performance depends on that of other components, often in subtle ways\.

### RPCs and commands \(request/confirm pairs\)

If a service [calls into another service](https://en.wikipedia.org/wiki/Remote_procedure_call) or requests it to act and return results \(this is how method calls are implemented in distributed systems\) it has to store the state of the scenario it is executing for the duration of the call \(until the confirmation message is received\)\. That uses resources: the stored state is kept in RAM and the interruption and resumption of the execution wastes CPU cycles on context switch and on the resulting cache misses\. Blocked threads are especially heavy while coroutines or fibers are more lightweight but are still not free\.

Another trouble with distributed systems comes from error recovery: if your component did not receive a timely response, you don’t know if your request was \(or is being, or will be\) executed by its target – and you need to be really careful about possible data corruption if you retry it and it is executed twice \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\.

<aside>

> If a request is duplicated \(as a slow network, overloaded service, or lost confirmation may cause a retry\), it is important to make sure that the second \(or parallel\) execution of the request does not change the system’s data\. This is achieved either by using [*idempotent*](https://en.wikipedia.org/wiki/Idempotence#Computer_science_examples) logic \(which is based on assignment instead of increasing or decreasing values in place\), or by writing the id of the last processed message to the database \(and checking that the incoming message’s id is greater than the one found in the database\) \[[MP](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.z69iqvut58vb)\]\.

</aside>

On the bright side, [*orchestration*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) is human\- and debugger\-friendly as it keeps consecutive actions close together in the code\. Therefore, synchronous interaction is the default mode of communication in many projects\.

### Notifications \(pub/sub\) and shared data

A service may do something, publish a notification or write results to a shared datastore for other services to process, and forget about the task as it has completed its role\. [*Choreography*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/choreography.md" >}}) is resource\-efficient, but you need to find and read multiple pieces of code which are spread out over several services to understand or debug the whole use case\.

### \(inexact\) No communication

Finally, some kinds of services, namely [*device drivers*]({{< relref "#inexact-device-drivers" >}}) and [*Nanoservices*]({{< relref "#inexact-nanoservices-api-layer" >}}), never communicate with each other\. Strictly speaking, such services don’t make a system – instead, they are isolated *Monoliths* which are managed by a higher\-level component \(OS kernel for drivers, client for *Nanoservices*\)\.

Nevertheless, it is a fun fact that if the services don’t intercommunicate, the main drawbacks of the *Services* architecture disappear:

- There is no slow and error\-prone interservice communication \(they never communicate\!\)\.
- It’s not hard to debug multi\-service use cases \(there are no such scenarios\!\)\.
- The services don’t corrupt data on crash \(there are no distributed transactions\)\.


## Variants by size

Last but not least, the simplest classification of subdomain\-separated components is by their size:

### Whole subdomain: \(Sub\-\)Domain Services

Each *Domain Service* \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] of [*Service\-Based Architecture*]({{< relref "#service-based-architecture" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] implements a whole subdomain\. It is the product of the full\-time work of a dedicated team\. A project is unlikely to have more than 10 of such services \(in part because the number of top\-level subdomains in any domain is usually limited\)\.

### Part of a subdomain: Microservices

*Microservices* enthusiasts estimate the best size of a component of their architecture to be below a month of development by a single team\. That allows for a complete rewrite instead of refactoring in case the requirements change\. When a team completes one microservice it can start working on another, probably related, one while still maintaining its previous work\. A project is likely to contain from tens to few hundreds of microservices\.

### Class\-like: Actors

An [*actor*](https://volodymyrpavlyshyn.medium.com/actors-actor-systems-as-massively-distributed-scalability-architecture-5e40f5ea9e86) is an object with a message\-based interface\. They are used correspondingly\. Though the size of an actor may vary, as does the size of an OOP class, it is still very likely to be written by a single programmer\.

### Single function: FaaS, Nanoservices

A [*nanoservice*](https://medium.com/@ido.vapner/unlocking-the-power-of-nano-services-a-new-era-in-microservices-architecture-22647ea36f22) is a single function \([FaaS](https://en.wikipedia.org/wiki/Function_as_a_service) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\]\) usually deployed to a *serverless* provider\. Nanoservices are used as API method handlers or as building blocks for [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\. 

## Variants by internal structure

A service is not necessarily monolithic inside\. Because a service is encapsulated from its users by its interface, it can have any kind of internal structure\. The most common cases, which can be intermixed together, are:

<figure style="text-align:center">
<a href="/Variants/1/Subtypes%20of%20Services.png" style="outline:none">
<img src="/Variants/1/Subtypes%20of%20Services.png" alt="Subtypes of Services" style="width:100%"/>
</a>
</figure>

### Monolithic service

<figure style="text-align:center">
<a href="/Variants/1/Service%20-%20Monolithic.png" style="outline:none">
<img src="/Variants/1/Service%20-%20Monolithic.png" alt="Service - Monolithic" style="width:27%"/>
</a>
</figure>

A *monolithic service* is a service with no definite internal structure, probably small enough to allow for complete rewrite instead of refactoring – the ideal of proponents of [*Microservices*]({{< relref "#microservices" >}})\. It is simple & stupid to implement but relies on external sources of persistent data\. For example, *device drivers* and [*Actors*]({{< relref "#actors" >}}) usually get their \(persisted\) configuration during initialization\. A monolithic backend service may receive all the data it needs in incoming requests, via a query to another service, or by reading it from a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}})\.

### Layered service

<figure style="text-align:center">
<a href="/Variants/1/Service%20-%20Layered.png" style="outline:none">
<img src="/Variants/1/Service%20-%20Layered.png" alt="Service - Layered" style="width:83%"/>
</a>
</figure>

A *layered service* is [divided into *layers*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}})\. This approach is very common both with backend *\(micro\-\)services*, where at least the database is separated from the business logic, and with *device drivers* in system programming, where hardware\-specific low\-level interrupt handlers and register access are separated from the main logic and high\-level OS interface\.

Layering provides all of the benefits from the [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) pattern, including support for [conflicting forces]({{< relref "../part-1--foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}), which may manifest, for example, as the ability to deploy the database to a dedicated server in backend or as a very low latency in the hardware\-facing layer of a device driver\.

Another benefit comes from the existence of the upper integration layer which may [orchestrate interactions with other services]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}}), isolating the lower layers from external dependencies\.

### Hexagonal service

<figure style="text-align:center">
<a href="/Variants/1/Service%20-%20Hexagonal.png" style="outline:none">
<img src="/Variants/1/Service%20-%20Hexagonal.png" alt="Service - Hexagonal" style="width:100%"/>
</a>
</figure>

A *hexagonal service* has its external dependencies isolated behind vendor\-agnostic interfaces\.

This is a real\-world application of [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) which both ensures that the business logic does not depend on specific technologies and protects from vendor lock\-in\. It is highly recommended for long\-lived projects\.

### Scaled service

<figure style="text-align:center">
<a href="/Variants/1/Service%20-%20Scaled.png" style="outline:none">
<img src="/Variants/1/Service%20-%20Scaled.png" alt="Service - Scaled" style="width:100%"/>
</a>
</figure>

With *scaled services* there are multiple [*instances*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of a service\. In most cases they [*share a database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) \(though sometimes the database may be [*sharded*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) or [*replicated*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) together with the service that uses it\) and get their requests through a [*Load Balancer* or *Sharding Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}})\.

### Cell \(WSO2 definition\) \(service of services\), Domain \(Uber definition\), Cluster

<figure style="text-align:center">
<a href="/Variants/1/Service%20-%20Cell.png" style="outline:none">
<img src="/Variants/1/Service%20-%20Cell.png" alt="Service - Cell" style="width:100%"/>
</a>
</figure>

When a service is split into a set of subservices, it makes a [*Cell*](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) \(WSO2 name\), [*Domain*](https://www.uber.com/blog/microservice-architecture/) \(Uber name\), or *Cluster* \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\. All the incoming communication passes through a [*Cell Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) which encapsulates the *Cell* from its environment\. Outgoing communication may involve the *Cell Gateway* or dedicated [*Adapters*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(*Anticorruption Layer* \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\]\) A *Cell* may deploy its own [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and/or [*share a database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) among its components\.

[*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \([according to WSO2](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md), as opposed to [Amazon's alias](https://docs.aws.amazon.com/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/what-is-a-cell-based-architecture.html) for [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}})\) appears when there is a need to recursively split a service, either because it grew too large or because it makes sense to use several incompatible technologies for its parts\. It may also be applied to group services if there are too many of them in the system\.

[*Domain\-Oriented Microservice Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}}) \(DOMA\) is a [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\-style layered system of *Cells*\.

## Examples

*Services* are pervasive among advanced architectures which either build around a layer of services that contains the bulk of the business logic \([*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\) or use small services as an extension of the main monolithic component \([*PlugIns*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) and [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}})\)\. [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}), [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) and [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) go all out partitioning the system into interconnected layers of services\. [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) and [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) require the services to implement or use a polymorphic interface to simplify the components that manage them\.

Examples of *Services* include:

### Service\-Based Architecture

<figure style="text-align:center">
<a href="/Variants/1/Service-Based%20Architecture.png" style="outline:none">
<img src="/Variants/1/Service-Based%20Architecture.png" alt="Service-Based Architecture" style="width:100%"/>
</a>
</figure>

This is the simplest use of *Services* where each subdomain gets a dedicated component\. A *Service\-Based Architecture* \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] tends to consist of a few coarse\-grained services, some of which may [*share a database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) and have little direct communication\. An [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) is often present as well\.

### Microservices

<figure style="text-align:center">
<a href="/Variants/1/Microservices.png" style="outline:none">
<img src="/Variants/1/Microservices.png" alt="Microservices" style="width:100%"/>
</a>
</figure>

*Microservices* \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] are usually smaller than components of *Service\-Based Architecture* and feature multiple services per subdomain with strict decoupling: no [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), independent \(and often dynamic\) scaling and deployment\. Even [*orchestration*]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) and distributed transactions \([*Sagas*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}})\) are considered to be a smell of bad design\.

*Microservices* fit loosely coupled domains with parts which [vary drastically](https://medium.com/swlh/stop-this-microservices-madness-8e4e0695805b) in both forces and technologies\. Any attempt to use them for an unfamiliar domain is [calling for trouble](https://martinfowler.com/bliki/MonolithFirst.html)\. Some authors insist that the “micro\-” means that a microservice should not be larger in scope than a couple of weeks of work for a programming team\. That allows rewriting one from scratch instead of refactoring\. Others assert that too high a granularity makes everything [overcomplicated](https://dwmkerr.com/the-death-of-microservice-madness-in-2018/)\. Such a diversity of opinions may mean that the applicability and the very definition of *Microservices* varies from domain to domain\.

This architecture usually relies on a [*Service Mesh*]({{< relref "../part-3--extension-metapatterns/combined-component.md#service-mesh" >}}) for [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) where common functionality, like logging, is implemented in co\-located [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\]\. A layer of [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) \(called *Integration Microservices*\) [may be present](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md), resulting in [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) or [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})\.

*Dynamically scaled* [*Pools*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) of service instances are common thanks to the elasticity of hosting in a cloud\. Extreme elasticity requires [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}), which puts a [distributed in\-memory database]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) node in each *Sidecar*\.

<aside>

> Some authors [distinguish](https://medium.com/@ali.gelenler/architectural-styles-vs-architectural-patterns-7fab51713470) between *architectural patterns* and *architecture styles* \(*architectures*\) \[[FSA](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#bookmark=kix.d09ykbr4tzvn), [MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\]\. The difference is similar to that between libraries and frameworks: you use a library or pattern \(e\.g\. division of a component into [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) or [*Services*](#)\) when you think that it will help your needs, but you build your entire system according to the rules of a framework or style \(such as *Microservices* or [*Enterprise SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}})\)\. This book does not accent that difference – instead, it boils down styles to combinations of patterns\.

</aside>

### Actors

<figure style="text-align:center">
<a href="/Variants/1/Actors.png" style="outline:none">
<img src="/Variants/1/Actors.png" alt="Actors" style="width:98%"/>
</a>
</figure>

An [*actor*](https://volodymyrpavlyshyn.medium.com/actors-actor-systems-as-massively-distributed-scalability-architecture-5e40f5ea9e86) is an entity with private data and a public message queue\. They are like objects with the difference that actors communicate only by sending each other asynchronous messages\. The fact that a single execution thread may serve thousands of actors makes actor systems an extremely lightweight approach to asynchronous programming\. As an actor is usually single\-threaded, there is no place for *mutexes* and *deadlocks* in the code and it is possible to [replay events](https://martinfowler.com/eaaDev/EventSourcing.html)\. Non\-blocking [*Proactors*]({{< relref "../part-2--basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) are often used in [real\-time systems]({{< relref "../part-1--foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}})\.

*Actors* have long been used in telephony \(which is the domain where real\-time communication meets complex logic and low resources\) and with the invention of distributed runtime environments \(e\.g\. Erlang/OTP or Akka\) they found their place in messengers and banking which need to interconnect millions of users while providing personalized experience and history for everyone\. Every user gets an actor that represents them in the system by communicating both with other actors \(forming a kind of [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\) and with the user’s client application\(s\)\.

If we apply a bit of generalization, we can deduce that any server or backend service is an actor because its data cannot be accessed from outside and asynchronous IP packets are its only means of communication\. Services of [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) closely match this definition\.

<aside>

> A [*deadlock*](https://en.wikipedia.org/wiki/Deadlock_(computer_science)) happens when several threads in a system wait for each other to release unique resources they have each taken\. As no thread involved in the *deadlock* can continue its operation, the system cannot complete its task\. A single\-threaded actor cannot *deadlock* because it does not contain multiple threads in the first place\.

</aside>

### \(inexact\) Nanoservices \(API layer\)

<figure style="text-align:center">
<a href="/Variants/1/Nanoservices%20-%20API%20Layer.png" style="outline:none">
<img src="/Variants/1/Nanoservices%20-%20API%20Layer.png" alt="Nanoservices - API Layer" style="width:82%"/>
</a>
</figure>

Though *Nanoservices* are defined by their size \(a single function\), not system topology, I want to mention a specific application from Diego Zanon’s book *Building Serverless Web Applications*\. That example is interesting because it comprises a single layer of isolated functions \(each providing a single API method\) which may share functionality by including code from a common repository\. As nanoservices of this kind never interact, the common drawbacks of *Services* \(poor debugging and high latency\) don’t apply to them\.

### \(inexact\) Device Drivers

<figure style="text-align:center">
<a href="/Variants/1/Drivers.png" style="outline:none">
<img src="/Variants/1/Drivers.png" alt="Drivers" style="width:100%"/>
</a>
</figure>

An operating system must run efficiently with an unpredictable combination of hardware components, any of which can come from different manufacturers\. It is impossible to know all the combinations beforehand\. Thus it employs one service \(called *driver*\) per hardware device\. A driver *adapts* a manufacturer\- and model\-specific hardware interface to the generic interface of the OS *kernel*, allowing for the kernel to operate the hardware it controls without the detailed knowledge of the model\. Internally, a driver is usually [*layered*]({{< relref "../part-2--basic-metapatterns/layers.md#embedded-systems" >}}):

- The lowest layer, called the [*Hardware Abstraction Layer*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(*HAL*\), provides a model\-independent interface for a whole family of devices from a manufacturer\. 
- The next layer of a driver is likely to contain manufacturer\-specific algorithms for efficient use of the hardware\.
- The third layer, if present, is probably busy with high\-level tasks which are common for all devices of the given type and may be implemented by the kernel programmers\.


The whole system of kernel, drivers, and user applications comprises the [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}}) architecture which bridges resource consumers and resource providers\. As the drivers don’t need to coordinate themselves \(this is done by the kernel\), they don’t really make a system of *Services* and thus don’t have the corresponding drawbacks\.

## Evolutions

*Services* are subject to a wide array of evolutions, just like the other basic metapatterns\. These are summarized below and detailed in [Appendix E]({{< relref "../part-7--appendices/appendix-e--evolutions/_index.md" >}})\.

### [Evolutions that add or remove services]({{< relref "../part-7--appendices/appendix-e--evolutions/evolutions-of-services-that-add-or-remove-services.md" >}})

*Services* work well when each service matches a subdomain and is developed by a single team\. If those premises change, you’ll need to restructure the services:

- A new feature request may emerge outside of any of the existing subdomains, creating a new service, or a service may grow too large to be developed by a single team, calling for division\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services_%20Split.png" style="outline:none">
<img src="/Evolutions/Services/Services_%20Split.png" alt="Services: Split" style="width:100%"/>
</a>
</figure>

- Two services may become so strongly coupled that they fare better if merged together, or the entire system may need to be glued back into a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) if the domain knowledge changes or if interservice communication strongly degrades performance\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services_%20Merge.png" style="outline:none">
<img src="/Evolutions/Services/Services_%20Merge.png" alt="Services: Merge" style="width:100%"/>
</a>
</figure>

### [Evolutions that add layers]({{< relref "../part-7--appendices/appendix-e--evolutions/evolutions-of-services-that-add-layers.md" >}})

The most common modifications of a system of *Services* involve supplementary system\-wide layers which compensate for the inability of the services to [share]({{< relref "../part-6--analytics/comparison-of-architectural-patterns/sharing-functionality-or-data-among-services.md" >}}) anything among themselves:

- A [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) tracks all the deployed service instances\. It mediates the communication between them and may manage their scaling and failure recovery\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services%20add%20Middleware.png" style="outline:none">
<img src="/Evolutions/Services/Services%20add%20Middleware.png" alt="Services add Middleware" style="width:100%"/>
</a>
</figure>

- [*Sidecars*]({{< relref "../part-3--extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] of a [*Service Mesh*]({{< relref "../part-3--extension-metapatterns/combined-component.md#service-mesh" >}}) make a virtual layer of shared libraries for the [*Microservices*]({{< relref "#microservices" >}}) it hosts\.


<figure style="text-align:center">
<a href="/Variants/2/Multifunctional%20-%20Service%20Mesh.png" style="outline:none">
<img src="/Variants/2/Multifunctional%20-%20Service%20Mesh.png" alt="Multifunctional - Service Mesh" style="width:100%"/>
</a>
</figure>

- A [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) simplifies the initial phases of development and interservice communication and enables the use of *Services* in [data\-centric domains]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/shared-data.md" >}})\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services%20to%20Shared%20Database.png" style="outline:none">
<img src="/Evolutions/Services/Services%20to%20Shared%20Database.png" alt="Services to Shared Database" style="width:100%"/>
</a>
</figure>

- [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) stand between the system and its clients and take care of shared aspects that otherwise would need to be implemented by every service\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services%20add%20Proxy.png" style="outline:none">
<img src="/Evolutions/Services/Services%20add%20Proxy.png" alt="Services add Proxy" style="width:100%"/>
</a>
</figure>

- An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) is the single place where the high\-level logic of all use cases resides\.


<figure style="text-align:center">
<a href="/Evolutions/Services/Services%20use%20Orchestrator.png" style="outline:none">
<img src="/Evolutions/Services/Services%20use%20Orchestrator.png" alt="Services use Orchestrator" style="width:100%"/>
</a>
</figure>

Those layers may also be consolidated into [*Combined Components*]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}):

- [*Message Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#message-bus" >}}) is a type of [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) that supports multiple protocols\.
- [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) combines [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(a kind of [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\) and [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.
- [*Event Mediator*]({{< relref "../part-3--extension-metapatterns/combined-component.md#event-mediator" >}}) is an [*orchestrating*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.
- [*Shared Event Store*]({{< relref "../part-3--extension-metapatterns/combined-component.md#persistent-event-log-shared-event-store" >}}) combines [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) and [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\.
- [*Enterprise Service Bus \(ESB\)*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) is an [*orchestrating*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) [*Message Bus*]({{< relref "../part-3--extension-metapatterns/combined-component.md#message-bus" >}})\.
- [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) employs all four layers: [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), and [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.


### Evolutions of individual services

Each service starts as either a [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) or as [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) and may undergo the corresponding evolutions:

- [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) help to reuse third\-party components \(e\.g\. a database\), organize the code, support conflicting forces and the upper layer of the service may [orchestrate other services]({{< relref "../part-1--foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}})\.
- A [*Cell*]({{< relref "#cell-wso2-definition-service-of-services-domain-uber-definition-cluster" >}}) is a service which is subdivided into several services that share an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/combined-component.md#api-gateway" >}}) and may [share a database]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) and/or a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. All of the components of a *Cell* are usually deployed together\. That helps when dealing with overgrown services without increasing the operational complexity of the system – but only if the *Cell*’s components are loosely coupled\.
- A service may use a [*Load Balancer*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) or a load balancing [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) to scale\. Its [*instances*]({{< relref "../part-2--basic-metapatterns/shards.md#stateless-pool-instances-replicated-stateless-services-work-queue" >}}) usually rely on a [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) for persistence\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) or [*CQRS*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) may be used inside a service to improve the performance of its data layer\.
- [*CQRS Views*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] or a [*Query Service*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#mp" >}})\] help reconstruct the state of other services from *event sourcing*\.
- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) isolates the business logic of the service from external dependencies\.
- In rare cases [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) or [*Scripts*]({{< relref "../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) help to vary the behavior of a service\.


## Summary

*Services* deal with large projects by dividing them into subdomain\-aligned components of smaller sizes which can be handled by dedicated teams\. These may vary in technologies and quality attributes\. However, services have a hard time cooperating in anything, from sharing data to debugging, and come with an innate performance penalty\. There are a few options halfway between [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) and *Distributed Services* that have milder benefits and drawbacks\.

<nav>

| \<\< [Layers]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) | ^ [Part 2\. Basic metapatterns]({{< relref "../part-2--basic-metapatterns/_index.md" >}}) ^ | [Pipeline]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) \>\> |
| --- | --- | --- |

</nav>



