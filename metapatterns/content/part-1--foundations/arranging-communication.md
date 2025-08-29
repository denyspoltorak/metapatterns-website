+++
weight = 4
title = "Arranging communication"
+++

# Arranging communication

As a project grows, it tends to become subdivided into services, modules, or whatever you call the components that match its subdomains \(or *bounded contexts*, if you prefer the \[[DDD]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\] convention\)\. Still, there remain system\-wide use cases that require collaboration from many or all of the system’s parts – otherwise the components don’t make a single system\. Let’s see how they can be integrated\.

<p align="center">
<img src="/Communication/Monolith to Services.png" alt="Monolith to Services" width=100%/>
</p>

As integration is not unique to distributed systems – it is present even in smaller programs that need to make data, functions, and classes work together – we’ll take a look at programming and architectural paradigms next\.

## Programming and architectural paradigms

Sharing a database is the greatest sin when you architect [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) yet [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}}) is built around shared data\. How do these approaches coexist? Do *Microservices* make any sense if blatantly violating their rules still results in successful projects?

Another programming paradox holds a clue\. There was C\. Then there came C\+\+ to kill C\. Then we’ve got Rust to kill C\+\+\. Now we have C, C\+\+, and Rust, all of them alive and kickin’\.

### Technologies are specialized

When a new technology emerges, it must show its superiority over existing mature methods\. In most cases that is achieved by specialization\. Is a car superior to a donkey? It depends\. Probably yes, when there are good roads, plenty of gas and spare parts\. A car is narrowly specialized, thus some areas have successfully adopted cars, while others still rely on donkeys\.

The same holds true for programming languages and architectures\. C is good when you work close to hardware and need complete control over whatever happens in the system\. C\+\+ is great at partitioning business logic, but it lost the simplicity of its predecessor\. Rust will likely shine in communication libraries, which are often targeted by hackers, though we have yet to see its wide adoption\. Hence the usefulness \(and choice\) of a tool or programming language depends on the circumstances\.

Let’s turn our attention to your average code\. It often mixes together:

- *Object\-oriented* programming that divides the application into a tree of loosely interacting pieces\.
- *Functional* programming, with the output of one function becoming the input to another, [method chaining](https://en.wikipedia.org/wiki/Method_chaining) included\.
- *Procedural* programming, where multiple functions access the same set of data, which also happens inside classes whose many methods operate their private data members\.


Each [programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) fits its own kind of tasks\. Moreover, the same three approaches reemerge at the system level:

### Object\-oriented \(centralized, shared nothing\) paradigm – orchestration

Almost every software project is too complex for a programmer to keep all the details of its requirements and implementation in their mind\. Notwithstanding, those details must be written down and run as code\.

The good old way out of the trouble is called [*divide and conquer*](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm)\. The global task is divided into several subtasks, and each subtask is subdivided again and again – till the resulting pieces are either simple enough to solve directly or [too messy]({{< relref "../part-1--foundations/modules-and-complexity.md" >}}) to allow for further subdivision\. Basically, we need to split our domain’s *control*, *logic*, and *data* into a single hierarchy of moderately sized components\.

We have heard a lot about keeping *logic and data* together: an object \(or actor, or module, or service – no matter what you call it\) must own its data to assure its consistency and hide the complexity of the component’s internals from its users\. If the encapsulation of an object's data is violated, the object’s code can neither trust nor restructure it\. On the other hand, if the data is bound to the logic that deals with it, the entire thing becomes a useful black box one does not need to look into to operate\.

Adding *control* to the blend is more subtle, but no less crucial than the encapsulation discussed above\. If an object commands another thing to do something, it must receive the result of the delegated action to know how to proceed with its own task\. Returning control after the action is conducted enables separation of high\-level supervising \(orchestration, integration\) logic from low\-level algorithms which it drives, adding depth to the structure\.

<p align="center">
<img src="/Communication/Paradigms - Object-oriented.png" alt="Paradigms - Object-oriented" width=100%/>
</p>

The ability to address complex domains by reducing the whole to self\-contained pieces makes object\-oriented design ubiquitous\. This paradigm, when applied to distributed systems, gives birth to [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}), [*Orchestrated Services*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), and [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})\.

<p align="center">
<img src="/Communication/Paradigms - Object-oriented - Variants.png" alt="Paradigms - Object-oriented - Variants" width=100%/>
</p>

### Functional \(decentralized, streaming\) paradigm – choreography

Sometimes you don’t need that level of fine\-tuning for the behavior of the system you build – it operates as an [assembly line](https://en.wikipedia.org/wiki/Assembly_line) with high throughput and little variance: its logic is made of steps that resemble work stations along a [conveyor belt](https://en.wikipedia.org/wiki/Conveyor_belt) through which identically structured pieces of data flow, just like goods on the belt\. In that case there is very little to control: if an item is good, it goes further, otherwise it just falls off the line\. Here the *control* resides in the graph of connections*,* the domain *logic* is subdivided, while the *data* is copied between the components\.

<p align="center">
<img src="/Communication/Paradigms - Functional.png" alt="Paradigms - Functional" width=100%/>
</p>

Functional or pipelined design is famous for its simplicity and high performance as the majority of processing steps can be scaled\. However, its straightforward application lacks the depth needed for handling complex processes, which would translate into webs of relations between hundreds of functions present at the same level of design\. It is also inefficient for choose\-your\-own\-adventure\-style \([*control*]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}})\) systems where too many too short conveyor belts would be required, negating the paradigm’s benefits\. And it may not be the right tool for making small changes in large sets of data as you’ll likely need to copy the whole dataset between the constituent functions\.

In distributed systems the functional paradigm is disguised as [*Choreographed Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}), [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}}), and various [batch or stream]({{< relref "../part-2--basic-metapatterns/pipeline.md#variants-by-scheduling" >}}) processing \[[DDIA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#ddd" >}})\] [*Pipelines*]({{< relref "../part-2--basic-metapatterns/pipeline.md#pipes-and-filters-workflow-system" >}})\.

<p align="center">
<img src="/Communication/Paradigms - Functional - Variants.png" alt="Paradigms - Functional - Variants" width=100%/>
</p>

### Procedural \(data\-centric\) paradigm – shared data

The final approach is integration through data\. There are cases where the domain data and business logic differ in structure – you cannot divide your project into objects because each of the many pieces of its logic needs to access several \(seemingly unrelated\) parts of its data\.

<p align="center">
<img src="/Communication/Paradigms - Data-centric.png" alt="Paradigms - Data-centric" width=100%/>
</p>

In the data\-centric paradigm *logic* and *data* are structured independently\. In procedural programming, like in object\-oriented paradigm, *control* is implemented inside the logic, making the logic layer hierarchical \(*orchestrated*\)\. Another, much less common, option relies on *Observer* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] to provide data change notifications, resulting in decentralized \(*choreographed*\) application logic:

<p align="center">
<img src="/Communication/Paradigms - Data-centric - Notifications.png" alt="Paradigms - Data-centric - Notifications" width=100%/>
</p>

The data\-centric approach works well for moderately\-sized projects with a stable data model \(like reservation of seats in trains or game of chess\)\. The best\-known distributed data\-centric architectures include [*Services with a Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) and [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.

<p align="center">
<img src="/Communication/Paradigms - Data-centric - Variants.png" alt="Paradigms - Data-centric - Variants" width=100%/>
</p>

### Composite cases

The three programming paradigms tend to collaborate:

- An ordinary class is object\-oriented on the outside but procedural inside: each of its methods can access any of its private data members\. Moreover, code inside methods may chain function calls, locally applying the functional paradigm\.
- [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) tends to use *choreography* \(pub/sub\) between *Cells* \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] and *orchestration* or communication via a *shared database* inside them\.
- A system of [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) \(or [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}})\) may be integrated through both [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) and [*Shared Database*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#shared-database-integration-database-data-domain-database-of-service-based-architecture" >}}) \(or *processing grid* and [*data grid*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}), correspondingly\)\.


### Reality is more complex

We have reviewed a few cases directly supported by common programming languages\. However, there is a wide variety of possible combinations of \(at least\) the following dimensions, each making a unique programming paradigm:

- Synchronous \(method calls\) vs asynchronous \(messaging\), with closely related:
  - Imperative vs reactive\.
  - Blocking vs non\-blocking\.
- Centralized \(orchestrated\) vs decentralized \(choreographed\) flow\.
- Shared data \(tuple space\) vs [shared nothing](https://en.wikipedia.org/wiki/Shared-nothing_architecture) \(messaging\)\.
- Commands \(actors\) vs notifications \(agents\)\.
- One\-to\-one \(channels\) vs many\-to\-one \(mailboxes\) vs one\-to\-many \(multicast\) vs many\-to\-many \(gossip\) communication\.


Some of the combinations look impossible or impractical, others are narrowly specialized thus uncommon, while many more are commonplace\. Discussing all of them would require insights from people who have used them in practice and that would take a dedicated book\.

### Summary

We have deconstructed the most common programming paradigms into their driving forces and shown how those forces shape distributed architectures:

- An object\-oriented system relies on hierarchical decomposition of a complex domain, just like *SOA* and *Orchestrated \(Micro\-\)Services* do\.
- Functional programming streams data through a sequence of transformations, which is the idea behind *Choreographed Event\-Driven Architecture* and *Data Mesh*\.
- Procedural style lets any piece of logic access the entire project’s data, resembling *Space\-Based Architecture* and *Services with a Shared Database*\.


Now let’s examine each of these approaches in depth:

## Orchestration

The most straightforward way to integrate services is to add a coordinating layer \([*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) on top of them:

<p align="center">
<img src="/Communication/Services to Orchestrator.png" alt="Services to Orchestrator" width=100%/>
</p>

The good thing is that your *Orchestrator* has explicit code for every use case it covers and every running scenario gets an associated thread, coroutine, or object so that you are able to attach to the *Orchestrator* and debug any use case step by step\. Nor do you have to worry about keeping the state of the services consistent as they are passive with all the changes in the system being driven by the *Orchestrator*\. 

Orchestration is the default approach for single\-process \(desktop\) applications where it is faster to call into an orchestrated module and return than to send it a message\. However, in distributed systems orchestration doubles the communication overhead \(when compared to [choreography]({{< relref "#choreography" >}}) or [shared data]({{< relref "#shared-data" >}})\) as every method call into an orchestrated service uses two messages: request and confirmation\.

### Roles

In a backend which serves client requests an *Orchestrator* takes the role of *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] – a module that provides and implements a high\-level interface for a multicomponent system\. It sends requests to the underlying services and waits for their confirmations – the mode of action that can be wrapped in an [*RPC*](https://en.wikipedia.org/wiki/Remote_procedure_call) \(*remote procedure call*\)\. The state of each scenario that the facade runs resides in the associated thread’s or coroutine’s call stack \(for [*Reactor*]({{< relref "../part-2--basic-metapatterns/monolith.md#single-threaded-reactor-one-thread-one-task" >}}) \[[POSA2]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}})\] or [*Half\-Sync/Half\-Async*]({{< relref "../part-2--basic-metapatterns/monolith.md#inexact-half-synchalf-async-coroutines-or-fibers" >}}) \[[POSA2]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}})\] implementations, correspondingly\) or in a dedicated object \(for [*Proactor*]({{< relref "../part-2--basic-metapatterns/monolith.md#proactor-one-thread-many-tasks" >}}) \[[POSA2]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa1" >}})\]\)\.

<p align="center">
<img src="/Communication/Facade.png" alt="Facade" width=100%/>
</p>

A *Facade* also supports querying the services in parallel and collecting the data returned into a single message through the *Splitter* and *Aggregator* patterns of \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\. That reduces latency \(and resource consumption as the whole task is completed faster\) for [scatter or gather](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) requests when compared to sequential execution\.

<p align="center">
<img src="/Communication/Facade - Parallel.png" alt="Facade - Parallel" width=90%/>
</p>

Embedded and system programming – the areas that deal with automating [*control*]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) of hardware or distributed software – employ *Orchestrators* as *Mediators* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\]  – components that keep the state of the whole system \(and, by implication, any hardware it may manage\) consistent by enacting a system\-wide reaction to any observable change in any of the system’s constituents\. A mediator operates in non\-blocking, fire\-and\-forget mode which is more characteristic of choreography, to be discussed [below]({{< relref "#choreography" >}})\. This also means that you will not be able to debug a use case as a thread – because [there are no predefined scenarios in control software](https://medium.com/itnext/control-and-processing-software-9011fee8bc66)\!

<p align="center">
<img src="/Communication/Mediator.png" alt="Mediator" width=100%/>
</p>

Such a difference may be rooted in the direction of the control and information flow: in a backend it comes as a high\-level command while control systems react to low\-level events\.

### Dependencies

By default an *Orchestrator* depends on each service which it manages – that means that a change in a service’s interface or contract – caused by fixing a bug, adding a feature, or optimizing performance – requires corresponding changes in the *Orchestrator*\. That is acceptable as the *Orchestrator*’s client\-facing, high\-level logic tends to evolve much faster than the business rules of the lower layer of services, therefore the team behind the orchestrator, unrestricted by other components depending on it, will likely release way more often than any other team\. However, as the number of the managed services and the lengths of their APIs increase, so does the amount of information that the *Orchestrator*’s team must remember and the influx of changes they must integrate in their code\. For a large project the workload of supporting the *orchestration layer* may paralyze its development – that was a major reason behind the decline of [*Enterprise SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md#enterprise-soa" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] where [*ESB*]({{< relref "../part-3--extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) used to orchestrate all the interactions in the system, including those between domain\-level services and components of the utility layer\.

<p align="center">
<img src="/Communication/Orchestrator - Dependencies.png" alt="Orchestrator - Dependencies" width=100%/>
</p>

Another option, which appears in [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) and develops in [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}}) and [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) stems from [*dependency inversion*](https://en.wikipedia.org/wiki/Dependency_inversion_principle): the *Orchestrator* defines an [*SPI*](https://en.wikipedia.org/wiki/Service_provider_interface) \(*service provider interface*\) for every service\. That makes each service depend on the *Orchestrator* so that a single *Orchestrator*’s team does not need to follow updates of the multiple services’ APIs – instead it initiates the changes at its own pace\. However, with that approach the design of an SPI requires coordination from the teams on both sides of it and the once settled interface becomes hard to change\. The most famous example of modules that implement SPIs are OS drivers\.

<p align="center">
<img src="/Communication/Microkernel - Dependencies.png" alt="Microkernel - Dependencies" width=100%/>
</p>

Furthermore, some domains develop that idea into a [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}): when services implement related concepts, they may match a single SPI, making the *Orchestrator* simpler \(as there is no more need to remember multiple interfaces\)\. That is the case with telecom or payment gateways and it may also be found with trees of product categories in online marketplaces\.

<p align="center">
<img src="/Communication/Hierarchy - Dependencies.png" alt="Hierarchy - Dependencies" width=100%/>
</p>

All kinds of orchestration allow for an easy addition of new use cases which may even involve new services as that changes nothing in the existing code\. However, removing or restructuring \(splitting or merging\) previously integrated services requires much work within the orchestrator, except for in a *Hierarchy* where all the services implement the same interface which means that the code in the *Orchestrator* does not depend \(much\) on any specific child\.

<p align="center">
<img src="/Communication/Orchestrator add a Use Case.png" alt="Orchestrator add a Use Case" width=100%/>
</p>

### Mutual orchestration

In some systems there are several services that have their own kinds of clients \(for example, employees of different departments\)\. Each of the services tries hard to process its clients’ requests on its own but occasionally still needs help from other parts of the system\. This creates a paradoxical case where several services orchestrate each other:

<p align="center">
<img src="/Communication/Mutual Orchestration - 1.png" alt="Mutual Orchestration - 1" width=100%/>
</p>

As each of the services depends on the APIs of the others, any change to any interface or composition of such a system requires consent and collaboration from every team as it impacts the code of all the services\.

<p align="center">
<img src="/Communication/Mutual Orchestration - 2.png" alt="Mutual Orchestration - 2" width=100%/>
</p>

In real life services are likely to be layered, with their upper layers acting as both internal and external *Orchestrators*\. Layering isolates interdependencies to the relatively small application\-level components and resolves, to an extent, the seemingly counterintuitive case of mutual orchestration as now there is an explicit, though fragmented, system\-wide orchestration layer\.

<p align="center">
<img src="/Communication/Mutual Orchestration - 3.png" alt="Mutual Orchestration - 3" width=100%/>
</p>

<p align="center">
<img src="/Communication/Mutual Orchestration - 4.png" alt="Mutual Orchestration - 4" width=100%/>
</p>

### Summary

Orchestration represents use cases as a code, allowing for an orchestrated system to support many complex scenarios\. Dealing with errors is as trivial as properly handling exceptions\. This approach trades performance for clarity\.

## Choreography

Another integration option is to build a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) to pass every client’s request through a chain of components:

<p align="center">
<img src="/Communication/Services to Pipeline.png" alt="Services to Pipeline" width=100%/>
</p>

In that case there is no owner for *workflows* – each request is just a data packet which is transformed multiple times as it passes through the *Pipeline*\. Debugging is mostly limited to reading logs as there is no dedicated component to connect to for single\-step execution of a use case\. Nor is there a single piece of code to define each of the system\-wide scenarios – their logic emerges from the graph of event channels that connect services and from messages that each involved event handler sends\. Consistency of the services’ states is the responsibility of the services themselves as there is none supervising them\.

On the bright side, there is no communication overhead caused by response messages as there are no responses – the processing cost is one message per service, half of the cost for an orchestrated architecture\. Still, messages in choreographed systems tend to be longer than those used with [orchestration]({{< relref "#orchestration" >}}) as each message needs to carry the entire request’s state – there is no [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) to own the state and distribute parts of the payload among involved services\.

<p align="center">
<img src="/Communication/Pipeline Enricher.png" alt="Pipeline Enricher" width=96%/>
</p>

Latency may also be suboptimal as parallelizing execution of a request is easier said than done because there is no place \(*Aggregator* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\) to collect multiple related messages, which also means that there is no associated cost in resources \(RAM and CPU time\) for storing their fragments\. Please note that an *Aggregator*, when added, starts orchestrating the system – it stands between the client and services and meddles with the traffic and logic\. It spends resources to store the received messages for aggregation, and the messages start forming request/confirm pairs – which are characteristic of orchestration\.

<p align="center">
<img src="/Communication/Pipeline Not Parallel.png" alt="Pipeline Not Parallel" width=100%/>
</p>

Still another trouble with choreography comes from its weakness in error processing\. When a service in the middle of a request processing pipeline encounters an error, it cannot generate its normal output to be sent further downstream\. One option is to fill in a null \(or error\) value but then each receiver of the message should remember to check for null and know how to deal with an error\. Another way is adding a dedicated error channel for each service to push failed requests into, but that complicates the whole system’s structure\. Moreover, a failure in the middle of processing a request may cause the services to end up with inconsistent data if no special attention \(a new kind of request to compensate the original one\) is paid to roll back the partial change\. Please note that all of that is conveniently handled by an *Orchestrator*\.

<p align="center">
<img src="/Communication/Pipeline Error.png" alt="Pipeline Error" width=96%/>
</p>

### Early response

The ordinary mode of action for a pipeline – sending the final results of processing to the client – requires either for the tail of the pipeline to send data to its head or for existence of a stateful intermediate component – [*Gateway*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) – to receive the client’s request, forward it to the head of the pipeline, wait on the pipeline’s tail for the result of processing, and return it to the client\. That is necessary because a client would usually open a single connection which is impossible to share between multiple services, namely the \(receiving\) head and \(sending\) tail of the pipeline\.

<p align="center">
<img src="/Communication/Pipeline Gateway.png" alt="Pipeline Gateway" width=100%/>
</p>

The gateway, if used, may parallelize processing of [scatter or gather](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/scatter-gather.html) requests by turning into an [*API Gateway*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#api-gateway" >}}) which is a kind of *Orchestrator*\. Which means that the system changes its paradigm from choreography to orchestration\.

<p align="center">
<img src="/Communication/Gateway to API Gateway.png" alt="Gateway to API Gateway" width=100%/>
</p>

It is possible to avoid both adding a *Gateway* and having the cyclic dependency if clients don’t immediately need the final results of processing their requests\. In such a case the service which receives the original request does its \(first\) step of processing, sends the response to the client, and then notifies services down the pipeline\. Though such a use case seems to be unlikely, it happens in real life, for example, with pizza delivery\. As soon as a buyer fills in their contact details and pays for the food, the order can be confirmed and forwarded to the kitchen\. When it is ready it’s forwarded to the delivery, and finally the physical goods appear at the buyer’s door\.

<p align="center">
<img src="/Communication/Pipeline Early Response.png" alt="Pipeline Early Response" width=96%/>
</p>

*Early response* allows for choreography to shine in its purest form: with extensibility, high performance, but also high latency\. A similar approach may be used in [*Service\-Based Architecture*]({{< relref "../part-2--basic-metapatterns/services.md#service-based-architecture" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] \(aka *Macroservices*\) [for communication between the services](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography) \(*bounded contexts*\) if they only need to notify each other of events without waiting for responses\.

### Dependencies

A pipeline may be built with downstream or upstream dependencies or with a shared schema\.

If services communicate through commands, each service depends on all the direct destinations of its commands as it must know each of the APIs which it uses\. This mode of communication is mostly used with [*Actors*]({{< relref "../part-2--basic-metapatterns/services.md#class-like-actors" >}}) that power embedded, telecom, messengers, and some banking systems\. Downstream dependencies make it easy to add input chains \(upstream services that deal with new hardware or external components\) although changing anything at the output end of the pipeline is going to break the input parts that send messages to the component changed\.

<p align="center">
<img src="/Communication/Downstream Dependencies.png" alt="Downstream Dependencies" width=99%/>
</p>

Upstream dependencies come from the [publish/subscribe](https://en.wikipedia.org/wiki/Publish–subscribe_pattern) model where each service broadcasts notifications about what it has done to any interested subscriber\. This way of building systems engines [*Event\-Driven Architecture*]({{< relref "../part-2--basic-metapatterns/pipeline.md#choreographed-broker-topology-event-driven-architecture-eda-event-collaboration" >}}) \[[FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\] which is used in high\-load backends\. Extending or truncating an already implemented request processing tree is as easy as adding or removing subscribers to existing events but the creation of a new event source will require changes in the downstream components\. The easy addition of downstream branches supports new customer experiences and analytical features which business is hungry for\.

<p align="center">
<img src="/Communication/Upstream Dependencies.png" alt="Upstream Dependencies" width=99%/>
</p>

The final option is for the entire pipeline to use a uniform message format \([*Stamp Coupling*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#inexact-stamp-coupling" >}}) \[[SAHP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa5" >}})\]\) which often contains one dedicated field per service involved\. This way a service depends only on the message header \(with the list of the fields and a record id\) and the format of the single field it reads \(stores data\) or writes \(retrieves data as *Content Enricher* \[[EIP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#deds" >}})\]\)\. That works well with system\-wide queries but binds all the services to the schema of the message in a way similar to accessing a shared database \(to be discussed [below]({{< relref "#shared-data" >}})\)\. Such an architecture decouples the services to the extent that any of them can be freely added or removed, together with the message field\(s\) it fills or reads\.

<p align="center">
<img src="/Communication/Shared Message Format.png" alt="Shared Message Format" width=99%/>
</p>

<p align="center">
<img src="/Communication/Add Remove with Shared Message.png" alt="Add Remove with Shared Message" width=100%/>
</p>

A peculiar feature of choreography is the ability to cut and cross\-link pipelines with compatible interfaces by changing a single service \(or even system configuration if you build it with communication channels\)\. That gives it a lot of flexibility – as long as you can comprehend all the dependencies \(and channels\) in the system, which becomes non\-trivial as it grows\.

<p align="center">
<img src="/Communication/Cross-link Pipeline.png" alt="Cross-link Pipeline" width=99%/>
</p>

### Multi\-choreography

It is very common for a service to participate in multiple pipelines, especially if it owns a database – as there should be a use case which fills in the data and at least one other scenario which reads from that database\. Each pipeline makes the service depend on one or more interfaces it communicates with, which often belong to multiple services, coupling components of the system and making it impair future structural changes\.

<p align="center">
<img src="/Communication/Multi-choreography.png" alt="Multi-choreography" width=99%/>
</p>

### Summary

Overall, choreography seems to be a lightweight approach that prioritizes throughput over latency and is suitable for highly\-loaded scenarios of limited complexity\. However, a choreographed system will likely become unintelligible if it is made to support more than a few use cases\.

There is a decent [overview from Microsoft](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography)\.

## Shared data

The final approach is integration through shared data \([*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}})\):

<p align="center">
<img src="/Communication/Services to Shared Data.png" alt="Services to Shared Data" width=100%/>
</p>

The shared data is a “blackboard” available for each service to read from and write to\. It is passive \(as controlled by the services\) and does not contain any logic except for the data schema, which represents a part of the domain knowledge\. That makes communication through shared data the antipode of [orchestration]({{< relref "#orchestration" >}}), which also features a shared component, [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), which is, however, active \(controls services\) and contains business logic, not data\.

Shared data can be used for storage, messaging, or both:

### Storage

The most common case of shared data is storage \(usually a database, sometimes a file system\) for a \(sub\)domain that has functionally independent services which operate on a common dataset\. For example, a ticket purchase service and a ticket refund service share a database of ticket details\. The ticket purchase service reads in the available seats and fills in ticket data for purchases\. The ticket refund service should be able to find all tickets bought by a user and delete the user data from seats refunded\. The only communication between the purchase and refund services is the shared database of tickets or seats, so that one of them sees the changes made by the other the next time it reads the data\.

<p align="center">
<img src="/Communication/Purchase and Return.png" alt="Purchase and Return" width=100%/>
</p>

With this model the services don’t depend on each other – instead, they depend on the shared \(domain\) data format and the database technology\. Thus, it is easy to add, modify, or remove services but hard to change the shared data structure or, especially, the database vendor\.

<p align="center">
<img src="/Communication/Shared Data - Dependencies.png" alt="Shared Data - Dependencies" width=92%/>
</p>

<p align="center">
<img src="/Communication/Shared Data add a Service.png" alt="Shared Data add a Service" width=99%/>
</p>

Services usually need to coordinate their actions\. Commonly, services with a shared database rely on a messaging [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) for communication\. Users of our ticketing system will want to be notified \(through email, SMS or an instant message\) when a free seat that they are interested in appears\. We’re not going to complicate either of the existing services by integration with instant messengers, so we will create a new notification service, which must track each returned ticket to see if any user wants to buy it\. This is easily implemented by the return service publishing and the notification service subscribing to a ticket return event, mixing in a bit of choreography into our data\-centric backend\.

<p align="center">
<img src="/Communication/Notification to Notification.png" alt="Notification to Notification" width=99%/>
</p>

Another case is found with data processing pipelines where an element may periodically read new files from a folder or new records from a database table to avoid implementing notifications\. This increases latency and may cause a little CPU load when the system is idle, but is perfectly ok for long\-running calculations\.

<p align="center">
<img src="/Communication/Shared files.png" alt="Shared files" width=100%/>
</p>

Finally, there is the rarely used option of an external [*Scheduler*]({{< relref "../part-3--extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) which selects the services which should run based on the data available\. This is known as the [*Blackboard* pattern]({{< relref "../part-3--extension-metapatterns/shared-repository.md#blackboard" >}}) and [something similar]({{< relref "../part-2--basic-metapatterns/monolith.md#inexact-reactor-with-extractors-phased-processing" >}}) happens in 3D game engines\. The *Scheduler* \(which is an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}), by the way\) is needed when CPU \(or GPU or RAM\) resources are much lower than what the services would consume if all of them ran in parallel, thus they must be given priorities, and the priorities change based on the context which is regularly estimated from the system’s data\.

<p align="center">
<img src="/Communication/Blackboard.png" alt="Blackboard" width=100%/>
</p>

### Messaging

The other, not as obvious, use case for shared data is messaging, which is implemented by the sender writing to a \(shared\) queue \(or log\) while the recipient is waiting to read from it\. Queues can be used for any kind of messages: request/confirm pairs, commands, or notifications\. Each service may have a dedicated queue \(either input for commands mode or output for notifications\), a pair of queues \(messages from the service’s output are duplicated by an underlying distributed [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) to input queues of their destinations\), or there may be a queue per communication channel, or a single queue for the entire system \(or a global queue per message priority\) with each message carrying destination id \(for commands\) or topic \(for notifications\)\.

<p align="center">
<img src="/Communication/Queues.png" alt="Queues" width=100%/>
</p>

The use of shared data for messaging turns our datastore into a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\. The dependencies are identical to [those in choreography]({{< relref "#dependencies-1" >}}) – each service depends on the APIs of its destinations for commands or its sources for notifications\.

There should be a means for the recipient of a message to know about its arrival so that it starts processing the input\. Usually a messaging *Middleware* implements a receive\(\) method for the service to block on\. However, very low latency applications, like [HFT](https://en.wikipedia.org/wiki/High-frequency_trading), may [busy\-wait](https://en.wikipedia.org/wiki/Busy_waiting) by repeatedly re\-reading the shared memory so that the service starts processing the incoming data immediately on its arrival, bypassing the OS scheduler\. This is the fastest means of communication available in software\.

### Full\-featured

Finally, some \(usually distributed\) datastores implement data change notifications\. That allows for the services to communicate through the datastore in real\-time, removing both the need for an additional *Middleware* and interdependencies for the services\. Such a system follows the [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) pattern of \[[POSA4]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#posa3" >}})\] which was rectified as [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) \[[SAP]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#sahp" >}}), [FSA]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#eip" >}})\]\. In our example, the available seats notification service subscribes to changes in the seats data in the database – this way it does not need to be aware of the existence of other services at all\. We can also move the email notifications logic of the ticket purchase service into a separate component which would track purchases in the database and send a printable version of each newly acquired ticket to the buyer’s email address which can be found in the ticket details in the database\.

<p align="center">
<img src="/Communication/Notification inside the DB.png" alt="Notification inside the DB" width=99%/>
</p>

### Summary

Communication through shared data is best suited for data\-centric domains \(for example, ticket purchase\)\. It allows for the services to be unaware of each other’s existence, just as they are with orchestration, but the structure of the domain data becomes hard to change as it is referenced all over the code\. Shared data may also be used to implement messaging\.

## Comparison of the options

We have briefly discussed three approaches to communication: orchestration, choreography, and shared data\. Let’s recall when it makes sense to use each of them\.

[*Orchestration*]({{< relref "#orchestration" >}}) is built around use cases\. They are easy to program and add, no matter how complex they become\. Thus, if your \(sub\)domain is coupled, or your understanding of it is still evolving, this is the way to go, as you will be able to change the high\-level logic in any imaginable way because you express it as convenient imperative code\.

[*Shared data*]({{< relref "#shared-data" >}}) is all about… er… domain data\. If you really \(believe that you\) know your domain, and it deals with coupled data – this is your chance\. You may even add in an *Orchestrator* if there are use cases that involve multiple subdomains\. The business logic is going to be easy to extend while changes to the data schema are sure to cause havoc\.

[*Choreography*]({{< relref "#choreography" >}}) pays off for weakly coupled domains with a few simple use cases\. It has good performance and flexibility, but lacks the expressive power of orchestration and becomes very messy as the number of tasks and components grows\. It works best with independent teams and delayed processing – when users do not wait for the immediate results of their actions\.

There is advice [from Microsoft](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography) and \[[DEDS]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#dds" >}})\] which makes perfect sense: use choreography for communication between *bounded contexts* \(subdomains\) but revert to orchestration \(or maybe shared data\) inside each context\. Indeed, subdomains are likely to be loosely coupled while most user requests don’t traverse subdomain boundaries – which kindles hope that their interactions are few and not time\-critical\. If we follow the advice, we get [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \([WSO2 definition](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)\), which collects the best of two worlds: orchestration and/or shared data for strongly coupled parts and choreography between them\.

<p align="center">
<img src="/Communication/Cell-Based Architecture.png" alt="Cell-Based Architecture" width=100%/>
</p>

By the way, you could have noticed a few odd cases:

- An [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) in a [control system]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) does not run scenarios and its mode of action resembles choreography\.
- A choreographed system may use a [shared message format]({{< relref "../part-3--extension-metapatterns/shared-repository.md#inexact-stamp-coupling" >}}), which makes it resemble a system with shared data, even though no shared database is present\.
- A shared database may be used to [implement messaging]({{< relref "../part-3--extension-metapatterns/shared-repository.md#persistent-event-log-shared-event-store" >}}) for an orchestrated or choreographed system, effectively becoming a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}})\.


That likely means that our distinction between the modes of communication is a bit artificial and there may be a yet unknown deeper model to look for\.

| \<\< [Four kinds of software]({{< relref "../part-1--foundations/four-kinds-of-software.md" >}}) | ^ [Part 1\. Foundations]({{< relref "../part-1--foundations/_index.md" >}}) ^ | [Part 2\. Basic Metapatterns]({{< relref "../part-2--basic-metapatterns/_index.md" >}}) \>\> |
| --- | --- | --- |


