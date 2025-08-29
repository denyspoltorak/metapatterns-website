+++
weight = 5
title = "The heart of software architecture"
+++

# The heart of software architecture

As the visible world boils down to protons and neutrons which stick together in various combinations, so too does the entirety of software architecture as a field of endeavor grow from an interplay of *cohesion* and *decoupling*\.

## Cohesers and decouplers

Any project carries many constraints \(*forces*\), some of which want for certain parts of its code to be kept together \(*cohesive*\) while others push to have them torn apart \(*decoupled*\)\. Their balance and the resulting optimal architecture is very fluid as each of the forces in action depends on the current circumstances, the project’s history and its expected evolution\.

### Code structure and the level of pain

Let’s explore how a force influences the structure of a project\. Consider the *clarity of code* which determines *development velocity*:

<p align="center">
<img src="/Heart/Pain.png" alt="Pain" width=100%/>
</p>

When you have 10 lines of business logic, you are likely to write them down as a simple script\. Separating them into classes or deploying 5 services, each running 2 lines of code, is an overkill which would make the complexity of your infrastructure much higher than that of the task on hand\.

At 100 lines of code you are likely to be more comfortable with procedures or even classes to divide the code into, as keeping everything together starts to hurt\. You switch from the most cohesive implementation to another one, decoupled to an extent\. Though the latter is more complex at its core, it allows for less painful growth because it encompasses smaller components\.

A file of 5 000 lines is hard to read – you need to separate it into modules, each of which contains classes, which contain methods, which contain the code\. You are building yet another level of the hierarchy to keep the number of items in each piece \(lines in a method, methods in a class, classes in a module\) comfortably small\.

At about 100 000 lines you may start considering further separation of your project into services – as, you know, there are merge conflicts, or it takes a while to compile and test the whole codebase… anyway, at that point very few people comprehend the whole thing in detail, thus the benefits of having the entire codebase co\-located \([*monorepo*](https://en.wikipedia.org/wiki/Monorepo)\) are diminishing while new drawbacks emerge\.

### Building a hierarchy

As we see from the example above, code clarity favors *cohesiveness* \(everything together\) for smaller codebases but *decoupling* \(parts separated with narrow interfaces\) for larger projects\. We can state that the direction the force under review \(clarity\) pushes us in depends on the project’s size\. That is not a unique case – many forces work this way, resulting in the famous [*Monolith* vs *Microservices* complexity diagram](https://martinfowler.com/bliki/MicroservicePremium.html)\.

Such a behavior is common for forces and, by the way, it is also the case with editing sorted data\. Array is the most efficient data structure for a small collection \(up to about 1 000 elements\) while anything larger requires a hash map or [B\-tree](https://en.wikipedia.org/wiki/B-tree) \(hierarchy of arrays\)\. Just as a database splits oversized arrays because they are too slow to edit, the human mind is inefficient with large collections of similar items and wants them to be restructured into a hierarchy\. When we look into a service, we see only the classes it contains\. When we examine a class, we check the list of its methods, not those of surrounding classes\. And when we open a method, we try to understand how its lines of code work together\. This is the way humans fight complexity – by selecting a segment at one level of abstraction and ignoring everything around\.

<p align="center">
<img src="/Heart/Hierarchy.png" alt="Hierarchy" width=100%/>
</p>

A hierarchy inherently adds some inconvenience – traversing levels of a B\-tree slows down operations, and a project with many files takes time to grasp – which is why we avoid deep hierarchies in smaller projects \(or datasets\) – but that is still a very low cost for having any individual component \(a method, class or module in a project; an array in a B\-tree\) stay reasonably small and simple thanks to the distribution of the overall complexity \(or data\) over the hierarchy\.

### Bidirectional forces

Among the forces that prefer decomposing a project into segments of certain size are:

- *Clarity* – as discussed above\.
- *Development velocity* – programmers are [very productive](https://realmensch.org/2018/05/04/we-are-all-10x-developers/) when they know their code and don’t waste their time on communication\. Still, there is a productivity limit for a single person, and if you want your project to be developed faster than that, you need to divide it among several teams, sacrificing individual performance for brute force\.
- *Latency* – it is poor for a distributed system, but is not much better for a large monolith that may deadlock\. Ideally, you should separate the latency\-critical part into a dedicated small component placed close to the input and output hardware\.
- *Throughput* – on one hand, interservice communication is suboptimal because of the associated networking and serialization\. On the other hand, there is a limit for a single server’s performance as more powerful hardware is too expensive\. It is cheaper to install several commodity servers and accept a communication penalty than keep the entire system running on a single high\-end machine to avoid networking\.
- *Security* – a single process is easier to audit and secure than a system of services\. However, as a service grows, it may develop multiple interfaces and assimilate many libraries\. Eventually, protecting that mishmash becomes much harder than creating a secure perimeter around a distributed system\.


### Cohesers

Other forces predominantly push you towards merging all your code and data together:

- *Debuggability* – it is hard to debug a distributed system\. You need to investigate logs and attach your debugger to several services which may be written in different programming languages\. Debugging a single process, even 10 MLoC in size, is almost always easier \(except when it is undocumented\)\.
- *Data consistency* – when everything runs in a single thread, you don’t have to care about data races, lost packets, or [idempotence](https://dev.to/woovi/idempotence-what-is-and-how-to-implement-4bmc)\. The [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) is on your side\.
- *Data analysis* – it is hard to collect data from multiple sources\. You cannot use SQL joins\. However, at some point in the distant future, you may still reach the performance limit of your single database, in theory making data analysis a kind of bidirectional force\.


### Decouplers

And there are forces that try to keep your code fragmented:

- *Variability* – if your project needs to satisfy many [conflicting requirements]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md" >}}), it is very hard to achieve that with a uniform codebase running in a single process\.
- *Location* – you may need to run parts of your system on [its end users’ devices]({{< relref "../part-1--foundations/forces--asynchronicity--and-distribution.md#distribution" >}}), in [regional data centers]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}), or even on [specialized hardware]({{< relref "../part-5--implementation-metapatterns/microkernel.md#autosar-classic-platform" >}})\.
- *Organizational structure* – according to [Conway’s law](https://en.wikipedia.org/wiki/Conway%27s_law), forcing everyone to work on a shared component is among the top team performance killers, especially when time zone differences are involved\.


### Expansion and contraction

<p align="center">
<img src="/Heart/Lifecycle.png" alt="Lifecycle" width=100%/>
</p>

As was [discussed previously]({{< relref "../part-6--analytics/architecture-and-product-life-cycle.md" >}}), when you start a project by building a [PoC](https://en.wikipedia.org/wiki/Proof_of_concept#Software_development) or prototype, you have little code and need to move quickly\. Most of the *decouplers* are not there and the *bidirectional forces* favor cohesion, thus you don’t waste your time on extra interfaces or fine\-grained services\. You don’t have multiple teams to fall prey to Conway’s law\.

As soon as the prototype is approved, it’s time to prepare for iterative development of a project which nobody comprehends in advance\. Requirements will change many a time\. Libraries and frameworks may not work as expected\. External services may die or change\. Your team members may leave or, worse, be eager to try a newer technology\. You need all the flexibility in the world, and the flexibility comes through decoupling\. But you also rely on your speed to remain ahead of competitors, therefore you cannot go too far because decoupling is not free\. You try to imagine what may change in the future and prepare by isolating the would\-be affected parts with well\-defined interfaces\.

As your software matures, the flow of changes slows down and the business becomes more predictable or, rather, more familiar\. You observe that some of the interfaces which you’ve added have never been put to real use – they just make the project more complex – you pay for decoupling without earning its benefits\. Others have been found to be too restrictive and were removed\. Thus the system begins to contract, burning the flexibility it no longer needs while also growing in directions that were not originally expected\.

Finally, you move to the support phase\. The best programmers leave for more active projects\. Whoever replaces them does not know the code\. The remaining flexibility decomposes in favor of quick hackarounds\. What remains is an ugly cohesive [evolutionary\-shaped mess](http://www.laputan.org/mud/), created through generations of ad\-hoc patches\.

## Deconstructing patterns

Imagine a dungeon with dragons\. It is made of halls connected by tunnels\. Each hall is *cohesive*\. Tunnels are narrow interfaces that *decouple* them\. A hall is amorphous – it can have any shape but it cannot open to another hall except through a tunnel – such are the rules of the game\. The tunnels both restrict the freedom of the halls and interconnect them\.

### SOLID principles

If *cohesion* and *decoupling* dictate software architecture, they should surface in its principles\. Let’s take a look at [SOLID](https://en.wikipedia.org/wiki/SOLID):

- The *single responsibility principle*, also known as [*do one thing and do it well*](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well), is a general advice for keeping unrelated functionality decoupled\.
- The *open\-closed principle* and *Liskov substitution principle* decouple the logic of the parent class or the code that uses it, correspondingly, from the functionality of its subclasses\.
- The *interface segregation principle* decouples independent parts of an object’s interface\.
- The *dependency inversion principle* decouples an object’s users from its implementation\.


Please beware that each of those principles in and of themselves involves decoupling which is not free – your software may end up having too many moving parts and strict rules to remain easy to read and support\.

> When we choose between cohesion and decoupling, we choose between a single component and a pair of components connected through a constraint rule\. The more decoupling, the more components and rules we have to handle\. Sooner, rather than later, the number of individual components and rules will overwhelm any developer\.

### Gang of Four patterns

Let’s now discuss something more practical, namely the \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] [patterns](https://en.wikipedia.org/wiki/Design_Patterns#Patterns_by_type) which seem to be ingenious but hacky ways for rearranging the roles in your code\. They override ordinary OOP rules, which is useful when you need extra flexibility\. For example, the *creational patterns* interfere with the normally cohesive *select type – create – initialize – use* sequence of operating an object\.

Some patterns provide a basic decoupling:

- *Adapter* translates between two interacting components so that they may evolve independently\.
- *Observer* decouples an event from the reactions it causes by registering handlers at runtime\.
- *Chain of Responsibility* separates method invocation from method execution\. A client’s calling a method of an object runs the corresponding method of another object\.


Others break the functionality or data of a class into two or more parts, juggling them at runtime:

- *Proxy* separates an object’s representation from its implementation, enabling lazy loading or remote access\.
- *Flyweight* segregates an immutable data member of a class to save memory by merging multiple instances of identical data\.
- *Strategy* and *Decorator* decouple a dimension of an object’s functionality to allow runtime changes in or composition of the object's behavior, respectively\. 
- *State* separates an object’s behavior into multiple classes based on the object’s state\.
- *Template Method* decouples several aspects of a class’s behavior from its main algorithm and envelops variations of those aspects into subclasses\.
- *Bridge* separates a high\-level hierarchy of classes from their low\-level implementation details which may comprise an orthogonal hierarchy\.
- *Memento* decouples the lifetime of an object’s state from the object itself\.


On the other hand, a few patterns gather separate components together:

- *Command* collects all the data required to call a method\.
- *Mediator* is a cohesive implementation of multi\-object use cases\.
- *Composite* and *Facade* represent multiple objects as a cohesive entity\. A *Composite* broadcasts a call to its interface to every object it contains while a *Facade* [orchestrates]({{< relref "../part-1--foundations/arranging-communication.md#orchestration" >}}) the wrapped subsystem\.
- *Abstract Factory* and *Builder* encapsulate *type selection* and *initialization* for several related hierarchies, so that the client code gets objects from a set of consistent types\. On top of that, a *Builder* cross\-links the objects it creates into a cohesive subsystem, which is returned to the *builder*’s client as a whole\.


The remaining patterns pick an aspect or two of an object’s behavior and move them elsewhere:

- *Iterator* moves the code for traversal of a container’s elements from the container’s clients into the container’s implementation, decoupling its clients from the iteration algorithm\.
- *Visitor* collects actions that a client needs to perform on each kind of object in a hierarchy, decoupling them from the classes that constitute the hierarchy\.
- *Interpreter* decouples client scenarios from the rest of the system by having them written in a dedicated language and run in a protected environment\.
- *Prototype* binds the *type selection* and *initialization* together and decouples them from the object *creation*\.
- *Singleton* binds the *creation* and *initialization* of a global object to every call of its methods\.
- *Factory Method* decouples the *initialization* from *type selection* and hides both from the class’s users\.


As we see, every \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] pattern boils down to binding \(making *cohesive*\) and/or separating \(*decoupling*\) some kind of functionality or responsibilities\.

### Architectural metapatterns

Finally, let’s close the book by iterating over the metapatterns and looking into their roots through the lens of unification and separation\.

<p align="center">
<img src="/Heart/Basic.png" alt="Basic" width=100%/>
</p>

Basic architectures:

- [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}) keeps everything together for quick and dirty projects:
  - Total *cohesiveness* results in low latency, cost\-efficient performance, and easy debugging\.
- [*Shards*]({{< relref "../part-2--basic-metapatterns/shards.md" >}}) slice a large\-scale application into multiple instances:
  - *Decoupling* the instances enables scaling but sacrifices consistency of shared data\.
- [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) separate the high\-level code from low\-level implementation:
  - *Cohesion* within a layer makes it easy to implement and debug\.
  - *Decoupled* layers may vary in technologies and properties but are somewhat slower and hard to debug in\-depth\.
- [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}) divide a complex system into subdomains:
  - *Cohesiveness* of a service keeps it simple and efficient when it does not need to consult with other services\.
  - *Decoupling* enables development of larger codebases by multiple specialized teams but global use cases become complicated\.
- [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}) segregates data processing into self\-contained steps:
  - *Decoupling* simplifies reassembling or expanding the system but increases its latency\.


<p align="center">
<img src="/Heart/Extension.png" alt="Extension" width=100%/>
</p>

Grouping related functionality:

- [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) separates the implementation of communication and/or instance management from the business logic:
  - The *cohesive* communication layer is reliable and uniform, thus it is easy to learn\.
  - *Decoupling* communication concerns from the business logic simplifies the latter\.
- [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}) dissociates data from code, enabling [data\-centric programming]({{< relref "../part-1--foundations/arranging-communication.md#shared-data" >}}):
  - *Cohesive* data is consistent and easy to handle\.
  - *Decoupled* business logic can be scaled or subdivided [independently]({{< relref "../part-1--foundations/arranging-communication.md#procedural-data-centric-paradigm--shared-data" >}}) of the data\.
- [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) mediates between a system and its clients, taking care of one or more aspects of their communication:
  - A *cohesive* edge component is easier to manage and secure\.
  - *Decoupling* generic aspects simplifies business logic but usually increases latency\.
- [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}}) collects a multitude of complex use cases into a dedicated layer:
  - *Cohesive* use cases are easy to comprehend and debug\.
  - *Decoupling* use cases from domain logic allows for variation in technologies but increases latency and complicates in\-depth debugging\.
- [*Combined Component*]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}) blends two or three of the above layers:
  - *Cohesion* improves performance but reduces flexibility\.


<p align="center">
<img src="/Heart/Fragmented.png" alt="Fragmented" width=100%/>
</p>

Decoupled systems:

- [*Layered Services*]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) first decouple the subdomains, and then the layers within each subdomain:
  - *Decoupled* subdomains allow for multi\-team development and large codebases but complicate global use cases\. *Decoupled* layers enable variation in technologies within a subdomain and [limit interdependencies]({{< relref "../part-1--foundations/arranging-communication.md#mutual-orchestration" >}}) between subdomains to a single layer\.
- [*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}}) divides data among multiple data stores:
  - *Decoupling* improves performance through database specialization at the cost of consistency\.
- [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) dedicate one or two components \(a [*Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\) per each kind of client\.
  - *Decoupling* allows for customization on a per\-client\-type basis but makes it hard to share functionality among the clients\.
- [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) first segregates a large system into layers, then subdivides each layer into services:
  - *Decoupling* layers strangely enables reuse as any component of an upper layer can access every component below it\. *Decoupling* services within the layers allows for multi\-team development\. Drawbacks include high latency, system complexity, and interdependencies\.
- [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) recursively separates general and specialized logic, tackling complexity:
  - *Cohesive* general and subdomain\-specific business logic helps readability and debugging\.
  - *Decoupled* layers and subdomains allow for modification and expansion of local functionality at the cost of performance\.


<p align="center">
<img src="/Heart/Implementation.png" alt="Implementation" width=100%/>
</p>

Component implementation:

- [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}}) separate customizable aspects of a system’s behavior:
  - *Decoupling* several aspects of a system allows for it to be fine\-tuned but requires careful design and may lower performance\.
- [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) isolates the business logic from its external dependencies:
  - *Decoupling* protects from vendor lock\-in and supports automatic testing at the cost of lost optimization opportunities\.
- [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}}) mediates between resource consumers and resource producers:
  - *Cohesive* resource management optimizes resource usage\.
  - *Decoupling* allows for seamless replacement of resource providers\.
- [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}}) aggregates distributed components into a virtual layer:
  - Virtual *cohesion* hides the complexity of distributed communication from client code\.
  - Actual *decoupling* \(distribution\) of the nodes enables scaling and fault tolerance\.


## Choose your own architecture

Now that we’ve seen patterns decomposed into decoupling and cohesion, we can try reconstructing architecture based on your project’s needs\.

### Project size

The project’s expected size is among the main determinants of the project’s architecture as both overgrown components and excessive fragmentation handicap development and maintenance\. A moderate number of components of moderate size is the desired zone of comfort\.

Therefore, a one day task will likely be [*monolithic*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), a man\-month of work needs [*layering*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) while anything larger calls for at least partial separation into *subdomain* [*modules*]({{< relref "../part-2--basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) or [*services*]({{< relref "../part-2--basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}})\. Very large projects may require further subdivision into [*Service\-Oriented Architecture*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) \(*SOA*\) or [*Cell\-Based Architecture*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) \(a kind of [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\)\.

<p align="center">
<img src="/Heart/Size-1.png" alt="Size-1" width=100%/>
</p>

Any inherent decoupling within your domain is another factor to consider in the initial design\. For example, the layer with domain logic is very likely to contain independent subdomains which naturally make modules or services at next to no development or runtime cost\. Likewise, [*Top\-Down Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) is a good fit for a hierarchical domain\. A domain that builds around stepwise processing of data or events may be modeled as a [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}), which is a very flexible architectural style\.

<p align="center">
<img src="/Heart/Size-2.png" alt="Size-2" width=100%/>
</p>

The number of teams you start the project with is also important\. For the teams to be as efficient as possible you want them to be almost independent\. As every team gets ownership of one or two components, you must assure that the architecture has enough modules or services for the teams to specialize, because anything shared will likely become a bottleneck\. For example, you can hardly employ more than 3 teams with a [*layered architecture*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) as there are only so many layers in any system\. Thus, having a large number of teams strongly hints at [*Services*]({{< relref "../part-2--basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}}), [*SOA*]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}), or [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})\.

If not all the teams are available from day one, it is still preferable to initially set up component boundaries for the prospective number of teams because subdividing an already implemented component is a terrible experience\. However, it may be [easier and safer](https://martinfowler.com/bliki/MonolithFirst.html) for now to leave all the components running within a single process \(as [*modules*]({{< relref "../part-2--basic-metapatterns/services.md#asynchronous-modules-modular-monolith-modulith-embedded-actors" >}})\) to avoid the overhead of going distributed and have less trouble moving pieces of code around as needed \(as new requirements often make a joke of your original design\)\. You should be able to make *modules* into [*services*]({{< relref "../part-2--basic-metapatterns/services.md#distributed-services-service-based-architecture-space-based-architecture-microservices" >}}) through moderate effort once that becomes an imperative\.

### Domain features

We’ve already seen above that hierarchical or pipelined domains enable the use of corresponding architectures\. There is more to it\.

Sometimes you expect to have many complex use cases which cannot be matched to your subdomains because every scenario involves multiple components, thus spreading over the entire system\. You would usually collect the global use cases into a dedicated component – an [*Orchestrator*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\. And if the *Orchestrator* grows out of control, it is [subdivided]({{< relref "../part-3--extension-metapatterns/orchestrator.md#variants-by-structure-can-be-combined" >}}) into layers or services\.

<p align="center">
<img src="/Heart/Features-1.png" alt="Features-1" width=100%/>
</p>

Other systems are built around data\. You cannot split it into private databases because almost every service needs access to the whole which necessitates a [*Shared Repository*]({{< relref "../part-3--extension-metapatterns/shared-repository.md" >}}), or the highly performant [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}})\.

<p align="center">
<img src="/Heart/Features-2.png" alt="Features-2" width=100%/>
</p>

Once you go distributed, you will likely employ a [*Middleware*]({{< relref "../part-3--extension-metapatterns/middleware.md" >}}) to centralize communication between your services\. And you will have various [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}}), such as a [*Firewall*]({{< relref "../part-3--extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}), a [*Reverse Proxy*]({{< relref "../part-3--extension-metapatterns/proxy.md#dispatcher-reverse-proxy-ingress-controller-edge-service-microgateway" >}}), and a [*Response Cache*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}})\. You may even deploy a *Proxy* per kind of client if the clients vary in protocols, resulting in [*Backends for Frontends*]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) \(*BFF*\)\.

<p align="center">
<img src="/Heart/Features-3.png" alt="Features-3" width=100%/>
</p>

### Runtime performance

Moreover, there are non\-functional requirements, such as performance and fault tolerance\.

High throughput is achieved by [*sharding*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-slice-sharding-shards-partitions-cells-amazon-definition" >}}) or [*replicating*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) your business logic or even your data\. Sharding also helps process huge datasets while replication improves fault tolerance\. [*Space\-Based Architecture*]({{< relref "../part-3--extension-metapatterns/combined-component.md#middleware-of-space-based-architecture" >}}) replicates the entire dataset in memory for faster access\.

<p align="center">
<img src="/Heart/Performance-1.png" alt="Performance-1" width=100%/>
</p>

Alternatively, you may use several specialized databases \([*Polyglot Persistence*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})\) or redesign a highly loaded part of your system as a self\-scaling [*Pipeline*]({{< relref "../part-2--basic-metapatterns/pipeline.md" >}})\.

<p align="center">
<img src="/Heart/Performance-2.png" alt="Performance-2" width=100%/>
</p>

Scalability under uneven load is achieved through [*Function as a Service*]({{< relref "../part-2--basic-metapatterns/services.md#single-function-faas-nanoservices" >}}) \(*Nanoservices*\), [*Service\-Mesh*]({{< relref "../part-3--extension-metapatterns/combined-component.md#service-mesh" >}})\-based [*Microservices*]({{< relref "../part-2--basic-metapatterns/services.md#microservices" >}}) and, to a greater extent, [*Space\-Based Architecture*]({{< relref "../part-5--implementation-metapatterns/mesh.md#space-based-architecture" >}})\.

<p align="center">
<img src="/Heart/Performance-3.png" alt="Performance-3" width=100%/>
</p>

Fault tolerance requires you to have [*replicas*]({{< relref "../part-2--basic-metapatterns/shards.md#persistent-copy-replica" >}}) of every component, including databases, ideally over multiple data centers\. If you are not that rich, be content with [*Actors*]({{< relref "../part-2--basic-metapatterns/services.md#actors" >}}) or [*Mesh*]({{< relref "../part-5--implementation-metapatterns/mesh.md" >}})\.

<p align="center">
<img src="/Heart/Performance-4.png" alt="Performance-4" width=100%/>
</p>

Low latency makes you place simplified first response logic close to your input, leading to:

- [*Model\-View\-Presenter*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}}) or [*Model\-View\-Controller*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) pattern families for user interaction\.
- [*Layers*]({{< relref "../part-2--basic-metapatterns/layers.md" >}}) with [*strategy injection*]({{< relref "../part-2--basic-metapatterns/layers.md#performance" >}}) for single hardware input\.
- A [*Hierarchy*]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}}) for distributed [*control*]({{< relref "../part-1--foundations/four-kinds-of-software.md#control-real-time-hardware-input" >}}) systems such as [IIoT](https://en.wikipedia.org/wiki/Industrial_internet_of_things)\.


<p align="center">
<img src="/Heart/Performance-5.png" alt="Performance-5" width=100%/>
</p>

### Flexibility

If your product needs customization, you go for [*Plugins*]({{< relref "../part-5--implementation-metapatterns/plugins.md" >}})\.

If it is to survive for a decade, you need [*Hexagonal Architecture*]({{< relref "../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) to be able to swap vendors\.

If you mediate between resource or service providers and consumers, you build a [*Microkernel*]({{< relref "../part-5--implementation-metapatterns/microkernel.md" >}})\.

<p align="center">
<img src="/Heart/Flexibility-1.png" alt="Flexibility-1" width=100%/>
</p>

When your teams develop services and you want them to be [less interdependent]({{< relref "../part-6--analytics/comparison-of-architectural-patterns.md#indirection-in-commands-and-queries" >}}), you insert an [*Anticorruption Layer*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), [*Open Host Service*]({{< relref "../part-3--extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), or [*CQRS View*]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) between them\.

<p align="center">
<img src="/Heart/Flexibility-2.png" alt="Flexibility-2" width=100%/>
</p>

When you have built a large system and really need that thorough data analytics, consider implementing a [*Data Mesh*]({{< relref "../part-2--basic-metapatterns/pipeline.md#data-mesh" >}})\.

<p align="center">
<img src="/Variants/1/Data Mesh.png" alt="Data Mesh" width=100%/>
</p>

### Every domain is unique

No one\-size\-fits\-all\. Embedded projects or single\-player games don’t have databases and run in a single process\. High Frequency Trading bypasses the OS kernel to save microseconds\. *Middleware* and distributed databases care about quorum and leader election\. Huge\-scale data processing must account for [bit flips](https://en.wikipedia.org/wiki/Soft_error)\. A medical device should never crash\. Banks store their history forever for external audits\.

There is no universal architecture\. No silver bullet pattern\. Patterns are mere tools\. Know your tools and choose wisely\.

### So it goes

Software architecture lies lifeless in my hands, devoid of its magical colors, *like the dead iguana*\.

| \<\< [Real\-world inspirations for architectural patterns]({{< relref "../part-6--analytics/real-world-inspirations-for-architectural-patterns.md" >}}) | ^ [Part 6\. Analytics]({{< relref "../part-6--analytics/_index.md" >}}) ^ | [Part 7\. Appendices]({{< relref "../part-7--appendices/_index.md" >}}) \>\> |
| --- | --- | --- |


