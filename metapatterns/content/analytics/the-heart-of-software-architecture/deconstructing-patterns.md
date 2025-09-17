+++
weight = 2
title = "Deconstructing patterns"
description = "Both SOLID principles, Gang of Four design patterns, and architectural metapatterns emerge from the interplay of coupling and cohesion."
images = ["/Heart/Basic.png"]
[sitemap]
  priority = 0.5
+++

# Deconstructing patterns

Imagine a dungeon with dragons\. It is made of halls connected by tunnels\. Each hall is *cohesive*\. Tunnels are narrow interfaces that *decouple* them\. A hall is amorphous – it can have any shape but it cannot open to another hall except through a tunnel – such are the rules of the game\. The tunnels both restrict the freedom of the halls and interconnect them\.

## SOLID principles

If *cohesion* and *decoupling* dictate software architecture, they should surface in its principles\. Let’s take a look at [SOLID](https://en.wikipedia.org/wiki/SOLID):

- The *single responsibility principle*, also known as [*do one thing and do it well*](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well), is a general advice for keeping unrelated functionality decoupled\.
- The *open\-closed principle* and *Liskov substitution principle* decouple the logic of the parent class or the code that uses it, correspondingly, from the functionality of its subclasses\.
- The *interface segregation principle* decouples independent parts of an object’s interface\.
- The *dependency inversion principle* decouples an object’s users from its implementation\.


Please beware that each of those principles in and of themselves involves decoupling which is not free – your software may end up having too many moving parts and strict rules to remain easy to read and support\.

<aside>

> When we choose between cohesion and decoupling, we choose between a single component and a pair of components connected through a constraint rule\. The more decoupling, the more components and rules we have to handle\. Sooner, rather than later, the number of individual components and rules will overwhelm any developer\.

</aside>

## Gang of Four patterns

Let’s now discuss something more practical, namely the \[[GoF]({{< relref "../../appendices/books-referenced.md#gof" >}})\] [patterns](https://en.wikipedia.org/wiki/Design_Patterns#Patterns_by_type) which seem to be ingenious but hacky ways for rearranging the roles in your code\. They override ordinary OOP rules, which is useful when you need extra flexibility\. For example, the *creational patterns* interfere with the normally cohesive *select type – create – initialize – use* sequence of operating an object\.

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
- *Composite* and *Facade* represent multiple objects as a cohesive entity\. A *Composite* broadcasts a call to its interface to every object it contains while a *Facade* [orchestrates]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) the wrapped subsystem\.
- *Abstract Factory* and *Builder* encapsulate *type selection* and *initialization* for several related hierarchies, so that the client code gets objects from a set of consistent types\. On top of that, a *Builder* cross\-links the objects it creates into a cohesive subsystem, which is returned to the *builder*’s client as a whole\.


The remaining patterns pick an aspect or two of an object’s behavior and move them elsewhere:

- *Iterator* moves the code for traversal of a container’s elements from the container’s clients into the container’s implementation, decoupling its clients from the iteration algorithm\.
- *Visitor* collects actions that a client needs to perform on each kind of object in a hierarchy, decoupling them from the classes that constitute the hierarchy\.
- *Interpreter* decouples client scenarios from the rest of the system by having them written in a dedicated language and run in a protected environment\.
- *Prototype* binds the *type selection* and *initialization* together and decouples them from the object *creation*\.
- *Singleton* binds the *creation* and *initialization* of a global object to every call of its methods\.
- *Factory Method* decouples the *initialization* from *type selection* and hides both from the class’s users\.


As we see, every \[[GoF]({{< relref "../../appendices/books-referenced.md#gof" >}})\] pattern boils down to binding \(making *cohesive*\) and/or separating \(*decoupling*\) some kind of functionality or responsibilities\.

## Architectural metapatterns

Finally, let’s close the book by iterating over the metapatterns and looking into their roots through the lens of unification and separation\.

<figure>
<a href="/diagrams/Heart/Basic.png" style="outline:none">
<img src="/diagrams/Heart/Basic.png" alt="Basic" style="width:100%"/>
</a>
</figure>

Basic architectures:

- [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}) keeps everything together for quick and dirty projects:
  - Total *cohesiveness* results in low latency, cost\-efficient performance, and easy debugging\.
- [*Shards*]({{< relref "../../basic-metapatterns/shards.md" >}}) slice a large\-scale application into multiple instances:
  - *Decoupling* the instances enables scaling but sacrifices consistency of shared data\.
- [*Layers*]({{< relref "../../basic-metapatterns/layers.md" >}}) separate the high\-level code from low\-level implementation:
  - *Cohesion* within a layer makes it easy to implement and debug\.
  - *Decoupled* layers may vary in technologies and properties but are somewhat slower and hard to debug in\-depth\.
- [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) divide a complex system into subdomains:
  - *Cohesiveness* of a service keeps it simple and efficient when it does not need to consult with other services\.
  - *Decoupling* enables development of larger codebases by multiple specialized teams but global use cases become complicated\.
- [*Pipeline*]({{< relref "../../basic-metapatterns/pipeline.md" >}}) segregates data processing into self\-contained steps:
  - *Decoupling* simplifies reassembling or expanding the system but increases its latency\.


<figure>
<a href="/diagrams/Heart/Extension.png" style="outline:none">
<img src="/diagrams/Heart/Extension.png" alt="Extension" style="width:100%"/>
</a>
</figure>

Grouping related functionality:

- [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) separates the implementation of communication and/or instance management from the business logic:
  - The *cohesive* communication layer is reliable and uniform, thus it is easy to learn\.
  - *Decoupling* communication concerns from the business logic simplifies the latter\.
- [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) dissociates data from code, enabling [data\-centric programming]({{< relref "../../foundations-of-software-architecture/arranging-communication/shared-data.md" >}}):
  - *Cohesive* data is consistent and easy to handle\.
  - *Decoupled* business logic can be scaled or subdivided [independently]({{< relref "../../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) of the data\.
- [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) mediates between a system and its clients, taking care of one or more aspects of their communication:
  - A *cohesive* edge component is easier to manage and secure\.
  - *Decoupling* generic aspects simplifies business logic but usually increases latency\.
- [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) collects a multitude of complex use cases into a dedicated layer:
  - *Cohesive* use cases are easy to comprehend and debug\.
  - *Decoupling* use cases from domain logic allows for variation in technologies but increases latency and complicates in\-depth debugging\.
- [*Combined Component*]({{< relref "../../extension-metapatterns/combined-component.md" >}}) blends two or three of the above layers:
  - *Cohesion* improves performance but reduces flexibility\.


<figure>
<a href="/diagrams/Heart/Fragmented.png" style="outline:none">
<img src="/diagrams/Heart/Fragmented.png" alt="Fragmented" style="width:100%"/>
</a>
</figure>

Decoupled systems:

- [*Layered Services*]({{< relref "../../fragmented-metapatterns/layered-services.md" >}}) first decouple the subdomains, and then the layers within each subdomain:
  - *Decoupled* subdomains allow for multi\-team development and large codebases but complicate global use cases\. *Decoupled* layers enable variation in technologies within a subdomain and [limit interdependencies]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md#mutual-orchestration" >}}) between subdomains to a single layer\.
- [*Polyglot Persistence*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md" >}}) divides data among multiple data stores:
  - *Decoupling* improves performance through database specialization at the cost of consistency\.
- [*Backends for Frontends*]({{< relref "../../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) dedicate one or two components \(a [*Proxy*]({{< relref "../../extension-metapatterns/proxy.md" >}}) and/or [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\) per each kind of client\.
  - *Decoupling* allows for customization on a per\-client\-type basis but makes it hard to share functionality among the clients\.
- [*Service\-Oriented Architecture*]({{< relref "../../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) first segregates a large system into layers, then subdivides each layer into services:
  - *Decoupling* layers strangely enables reuse as any component of an upper layer can access every component below it\. *Decoupling* services within the layers allows for multi\-team development\. Drawbacks include high latency, system complexity, and interdependencies\.
- [*Hierarchy*]({{< relref "../../fragmented-metapatterns/hierarchy.md" >}}) recursively separates general and specialized logic, tackling complexity:
  - *Cohesive* general and subdomain\-specific business logic helps readability and debugging\.
  - *Decoupled* layers and subdomains allow for modification and expansion of local functionality at the cost of performance\.


<figure>
<a href="/diagrams/Heart/Implementation.png" style="outline:none">
<img src="/diagrams/Heart/Implementation.png" alt="Implementation" style="width:100%"/>
</a>
</figure>

Component implementation:

- [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) separate customizable aspects of a system’s behavior:
  - *Decoupling* several aspects of a system allows for it to be fine\-tuned but requires careful design and may lower performance\.
- [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) isolates the business logic from its external dependencies:
  - *Decoupling* protects from vendor lock\-in and supports automatic testing at the cost of lost optimization opportunities\.
- [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) mediates between resource consumers and resource producers:
  - *Cohesive* resource management optimizes resource usage\.
  - *Decoupling* allows for seamless replacement of resource providers\.
- [*Mesh*]({{< relref "../../implementation-metapatterns/mesh.md" >}}) aggregates distributed components into a virtual layer:
  - Virtual *cohesion* hides the complexity of distributed communication from client code\.
  - Actual *decoupling* \(distribution\) of the nodes enables scaling and fault tolerance\.


<nav>

| \<\< [Cohesers and decouplers]({{< relref "../../analytics/the-heart-of-software-architecture/cohesers-and-decouplers.md" >}}) | ^ [The heart of software architecture]({{< relref "../../analytics/the-heart-of-software-architecture/_index.md" >}}) ^ | [Choose your own architecture]({{< relref "../../analytics/the-heart-of-software-architecture/choose-your-own-architecture.md" >}}) \>\> |
| --- | --- | --- |

</nav>