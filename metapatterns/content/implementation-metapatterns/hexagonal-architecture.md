+++
weight = 6
title = "Hexagonal Architecture"
description = "This chapter explores Hexagonal Architecture and its subtypes: Ports and Adapters, Onion Architecture, MVC, MVP and MVVM, Pedestal, and Cell (Cluster)."
images = ["/diagrams/Web/og/Hexagonal%20Architecture.png"]
[sitemap]
  priority = 0.8
+++

# Hexagonal Architecture {anchor=false}

<figure>
<a href="/diagrams/Main/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Main/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Hexagonal%20Architecture.png" alt="A diagram for Hexagonal Architecture, in abstractness-subdomain-sharding coordinates." loading="lazy" width="962" height="642" style="width:100%"/>
</picture>
</a>
</figure>

*Trust no one\.* Protect your code from external dependencies\.

<ins>Known as:</ins> Hexagonal Architecture, or originally as Ports and Adapters\.

<ins>Structure:</ins> A monolithic business logic extended with a set of \(adapter, component\) pairs that encapsulate external dependencies\.

<ins>Type:</ins> Implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Isolates business logic from external dependencies which become expendable | Suboptimal performance |
| The programmers of business logic don’t need to learn any external technologies | The vendor\-independent interfaces must be designed before the start of development |
| Facilitates the use of stubs/mocks for testing and development |  |
| Allows for qualities to vary between the external components and the business logic |  |

<ins>References:</ins> [Herberto Graça’s chronicles](https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/) is the main collection of patterns from this chapter\. *Hexagonal Architecture* has [the original article](https://alistair.cockburn.us/hexagonal-architecture/) and a brief summary of its layered variant in \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\]\. Most of the *Separated Presentation* patterns are featured on Wikipedia and there are collections of them from [Martin Fowler](https://martinfowler.com/eaaDev/uiArchs.html), [Anthony Ferrara](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html), and [Derek Greer](https://lostechies.com/derekgreer/2007/08/25/interactive-application-architecture/)\. *Cells* originate with [WSO2](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) and [Uber](https://www.uber.com/en-UA/blog/microservice-architecture/)\.

*Hexagonal Architecture* \(see the [original article](https://alistair.cockburn.us/hexagonal-architecture/) for the explanation of its name\) is a variation of [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) that aims for self\-sufficiency of a system’s business logic *core*\. It hides external components such as libraries, services or databases, which the business logic would normally depend upon, behind [*adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] that translate from the external component’s interface to an interface \([*API*]({{< relref "#upper-half-separated-presentation-open-host-service" >}}) or [*SPI*]({{< relref "#lower-half-pedestal-abstraction-layer-anticorruption-layer" >}})\) defined in terms of the business logic\. This indirection not only makes the bulk of the system’s code easy to port, test, and run in isolation, but also allows for replacing external components and for changing the interfaces of the business logic or even its internal structure late in the [project’s life cycle]({{< relref "../analytics/architecture-and-product-life-cycle.md" >}})\.

Though various kinds of *Hexagonal Architecture* are de facto an industry standard, they are not without drawbacks: the [extra layer of indirection](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering) requires careful planning with insights into how the system will evolve and which kinds of external components will need to be integrated or which platforms the system will need to run on\. It also adds boilerplate code and prevents aggressive performance optimization that would rely on peculiarities of an external component currently integrated\.

### Performance

*Hexagonal Architecture* is a strange beast performance\-wise\. The generic interfaces between the *core* and *adapters* stand in the way of whole\-system optimization and may add context switching\. Still, at the same time, each *adapter* concentrates all the vendor\-specific code for its external dependency, which makes the *adapter* a perfect single place for aggressive optimization by an expert or consultant who is proficient with the adapted third\-party software but does not have time to learn the details of your business logic\. Thus, some opportunities for optimization are lost while others emerge\.

Occasionally the system may benefit from direct communication between the *adapters*\. However, that requires several of them to be compatible or polymorphic, in which case your *Hexagonal Architecture* may in fact be a kind of shallow [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\. Examples include a service that uses several databases which are kept in sync through [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\) or a telephony gateway that interconnects various kinds of voice devices\.

<figure>
<a href="/diagrams/Performance/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Performance/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Hexagonal%20Architecture.png" alt="A data stream between adapters of Hexagonal Architecture." loading="lazy" width="703" height="421" style="width:92%"/>
</picture>
</a>
</figure>

### Dependencies

Each [*adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) breaks the dependency between the *core* that contains business logic and an adapted component\. This makes all the system’s components mutually independent – and easily interchangeable and evolvable – except for the *adapters* themselves, which are small enough to be rewritten as need arises\.

<figure>
<a href="/diagrams/Dependencies/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Dependencies/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Hexagonal%20Architecture.png" alt="In Hexagonal Architecture each adapter depends on the core and the component or protocol it adapts." loading="lazy" width="803" height="483" style="width:88%"/>
</picture>
</a>
</figure>

Still, there is a hidden pitfall of designing a [*leaky abstraction*](https://en.wikipedia.org/wiki/Leaky_abstraction) – an interface that looks generic but whose contract matches that of the component it encapsulates, making it much harder than expected to change the external component’s vendor\.

### Applicability

*Hexagonal Architecture* <ins>benefits</ins>:

- *Medium\-sized or larger components\.* The programmers don’t need to learn details of external technologies and may concentrate on the business logic instead\. The code of the *core* becomes smaller as all the details of managing external components are moved into their *adapters*\.
- *Cross\-platform development\.* The core is naturally cross\-platform as it does not depend on any \(platform\-specific\) libraries or frameworks\.
- *Long\-lived products*\. Technologies come and go, your product remains\. Always be ready to change the technologies it uses\.
- *Unfamiliar domain\.* You don’t know how much load you’ll need your database to support\. You don’t know if the library you selected is stable enough for your needs\. Be prepared to replace vendors even after the public release of your product\.
- *Automated testing\.* [Stubs and mocks](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) are great for reducing load on test servers\. And stubs for the [SPI]({{< relref "#lower-half-pedestal-abstraction-layer-anticorruption-layer" >}})s which you wrote yourself are easy as a pie\.
- *Zero bug tolerance\.* SPIs allow for event replay\. If your business logic is deterministic, you can [reproduce your user’s bugs in your office](http://ithare.com/chapter-vc-modular-architecture-client-side-on-debugging-distributed-systems-deterministic-logic-and-finite-state-machines/)\.


*Hexagonal Architecture* <ins>is not good</ins> for:

- *Small components\.* If there is little business logic, there is not much to protect, while the overhead of defining [SPI]({{< relref "#lower-half-pedestal-abstraction-layer-anticorruption-layer" >}})s and writing *adapters* for them is high compared to the total development time\.
- *Write\-and\-forget projects\.* You don’t want to waste your time on long\-term survivability of your code\.
- *Quick start*\. You need to show the results right now\. No time for good architecture\.
- *Low latency*\. The *adapters* slow down communication\. This is somewhat alleviated by creating [direct communication channels]({{< relref "#performance" >}}) between the *adapters* to bypass the *core*\.


### Relations

<figure>
<a href="/diagrams/Relations/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Relations/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Hexagonal%20Architecture.png" alt="Diagrams of Hexagonal Architecture with a monolithic core, with a layered core, and Cell." loading="lazy" width="1144" height="621" style="width:100%"/>
</picture>
</a>
</figure>

*Hexagonal Architecture*:

- Is a kind of [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}})\.
- May be a shallow [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\.
- Implements [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Extends [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}), or [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) with one or two layers of [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}})\.
- The [*MVC* family of patterns]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) is also [derived]({{< relref "../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) from [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.


## Variants by placement of adapters

There is a variation in a distributed or asynchronous *Hexagonal Architecture* regarding the deployment of [*adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}), which may reside with the components they adapt or adjacent to the *core*:

### Adapters on the external component side

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.png" alt="Adapters co-located with external components translate a single message from the core into multiple calls to the adapted components." loading="lazy" width="823" height="403" style="width:93%"/>
</picture>
</a>
</figure>

If your team owns the component adapted, the *adapter* may be placed next to it\. That usually makes sense because a single domain message \(designed in the terms of your business logic\) tends to unroll into a series of calls to an external component\. The fewer messages you send, the faster your system is\.

### Adapters on the core side

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.png" alt="Adapters for external services and a shared database are co-located with the core." loading="lazy" width="1063" height="324" style="width:100%"/>
</picture>
</a>
</figure>

Sometimes you need to adapt an external service which you don’t control\. In that case the only real option is to place its *adapter* together with your *core* logic\. For a monolithic *core* \([*Ports and Adapters*]({{< relref "#ports-and-adapters-hexagonal-architecture" >}})\) the *adapters* may run in the core’s process as ordinary modules\. A distributed *core* \([*Cell*]({{< relref "#cell-cluster-domain" >}})\) requires the *adapters* to be stand\-alone components, which however can be co\-located and co\-deployed with the *core* as [*Sidecars*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}})\.

## Variants by encapsulation

In most cases a component or subsystem is involved in two kinds of communication:

- Input/output from clients, users or network devices which we [draw on the upper side of diagrams]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}})\. These events initiate the application’s use cases thus they are called [*primary* or *driving*](https://alistair.cockburn.us/hexagonal-architecture)\.
- The activities and calls initiated by the component itself while executing a use case\. They usually target the infrastructure or external services, are called [*secondary* or *driven*](https://alistair.cockburn.us/hexagonal-architecture), and are drawn on the lower side\.


<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Driving%20and%20Driven.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driving%20and%20Driven.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driving%20and%20Driven.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Driving%20and%20Driven.png" alt="The system's core is isolated with adapters for the following communication: data stream, REST, and SSH inputs; database and library access; publish/subscribe output stream." loading="lazy" width="1083" height="583" style="width:100%"/>
</picture>
</a>
</figure>

<aside>

> Client output *adapters*, such as the *view* in [*MVC*]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) or *responder* in [*ADR*]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}), are treated as *primary* even though they are called by the business logic\. The reason is that such an *adapter* is a part of the client feedback loop which *drives* the system’s behavior\.

</aside>

We can protect a component from its environment by inserting *adapters* into its *primary*, *secondary*, or both communication pathways:

### Upper half: [Separated Presentation]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}), [Open Host Service]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}})

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Driving.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driving.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driving.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Driving.png" alt="A component provides an API which is called by a GUI adapter for a system's user, a REST adapter for a web application, and a gRPC adapter for a software client." loading="lazy" width="943" height="323" style="width:100%"/>
</picture>
</a>
</figure>

Isolation on the input \([*primary*, *driving*](https://alistair.cockburn.us/hexagonal-architecture)\) side is achieved by designing an *Application Programming Interface* \(*API*\) that provides access to your component’s use cases which your software exists to implement and writing *adapters* that translate your *API* into something convenient for every kind of communication used by your clients\. For example, you may need a desktop GUI, mobile GUI, CLI, web application \(REST\), and customer \(JSON or gRPC\) *adapters* all of which are built on top of your component’s API\.

Input side isolation allows for your system to be used in different environments and roles and enables automated testing of its business logic without hard\-to\-support GUI tests\.

<aside>

> Though it may seem that a system’s business logic cannot depend on its [*interface*]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) \(GUI or web API\) because it is the *interface* that calls the business logic, in fact the business logic must prepare the data it returns in the format declared by the interface \(for a web protocol\) or output the data queried to a widget \(for a GUI\)\. Therefore making the business logic independent requires an [*Adapter*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) to translate between the domain terms native to the business logic and a data format used by a web protocol or GUI framework which calls the business logic\.

</aside>

[*Separated Presentation*](https://martinfowler.com/eaaDev/SeparatedPresentation.html) protects business logic \(*model*\) from a dependency on a [*Presentation Layer*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] \(interactions with the system’s user via a window, command line, or web page\)\. There is a [great variety](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html) of such patterns, commonly known as *Model\-View\-Controller* \(*MVC*\) alternatives which make three structurally distinct groups:

- Bidirectional flow – the *view* \(user\-facing component\) both receives input and produces output and there is often an extra *adapter* between it and the main system, resulting in [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\. These patterns make the [*Model\-View\-Presenter* \(*MVP*\) family]({{< relref "#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}})\.
- Unidirectional flow – the *controller* receives input while the *view* only produces output, [forming]({{< relref "../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md#model-view-controller-mvc" >}}) a kind of [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\. Such patterns are [associated with *Model\-View\-Controller* \(*MVC*\)]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}})\.
- Hierarchical patterns with multiple *models*, [discussed]({{< relref "../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) in the [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) chapter\.


*Open Host Service* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] is an enterprise pattern about providing functionality for use by other components \(hence the name\) through publishing a stable interface, known as *Published Language* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\. As the subject service should be able to change, and as its clients may differ in their needs, requiring multiple *Published Languages*, an *Open Host Service* usually involves an *adapter* for every *Published Language* it provides – just like *Separated Presentation* does for supported *UI*s\.

<aside>

> As is common with patterns, there is a slight difference between what the client\-facing side includes in *Separated Presentation* and *Open Host Service*, with the last pattern including adapters for *Publish\-Subscribe* interfaces\. Neither of these two patterns provides indirection for input data or event streams\.

</aside>

### Lower half: [Pedestal]({{< relref "../basic-metapatterns/services.md#inexact-device-drivers-pedestal" >}}), [Abstraction Layer, Anticorruption Layer]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}})

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Driven.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driven.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Driven.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Driven.png" alt="The core uses adapters to call a database, a library, and an external service." loading="lazy" width="843" height="307" style="width:93%"/>
</picture>
</a>
</figure>

In many cases you need to protect your component from its dependencies\. If the bulk of your code depends on a certain external service provider or component, you face a major risk, called [*vendor lock\-in*](https://en.wikipedia.org/wiki/Vendor_lock-in), of that provider going out of business, being acquired and killed by a larger company, or greatly increasing the price of its services\. Therefore you better define a *Service Provider Interface* \(*SPI*\) in terms of your business logic and write an *adapter* between it and the external provider’s API\. This way you will be able to change the external provider by merely rewriting the *adapter*\. It will also be easy to upgrade to new versions of the provider’s API\. The set of *adapters* to the services used by a component is called *Anticorruption Layer* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] because it protects the component from any external changes\.

There is a similar situation in embedded and systems programming where software faces hardware\. A hardware component is normally in production only for a few years before being replaced by a newer, and often incompatible, chip\. You have to make sure that your business logic can run on any hardware that provides the functions it needs – therefore you write hardware\-specific [*drivers*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) which adapt the target hardware’s interface to the expectations of your business logic\. The *drivers* make an [*Hardware Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) while the entire architecture is called a [*Pedestal*]({{< relref "#pedestal" >}})\.

Another common case is platform independence – you may need to run your code under different operating systems or cloud orchestrators, which requires an [*OS Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(*OSAL*\) or a [*Platform Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(*PAL*\)\. And you may even use a [*Database Abstraction Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(*DAL*\) to feel more secure\.

<aside>

> Embedded software and operating systems [don’t run use cases]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md#control-real-time-hardware-input" >}}) – they react to events which come from hardware\. This fact breaks the *Hexagonal Architecture*’s model of [*driving* and *driven* adapters]({{< relref "#variants-by-encapsulation" >}}) – any hardware component can *drive* a part of the system’s behavior but none of them initiates use cases\.

</aside>

The extra benefit of the isolation from infrastructure and external services by using [*secondary* or *driven*](https://alistair.cockburn.us/hexagonal-architecture) *adapters* is the ability to run against [*test doubles*](https://martinfowler.com/bliki/TestDouble.html) – local [*stubs* or *mocks*](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) that replace an external dependency, saving testing time and money\. They also allow for implementing business logic in parallel with benchmarking the external services which it will use and even replacing a service provider late in the [project’s life cycle]({{< relref "../analytics/architecture-and-product-life-cycle.md" >}})\.

<aside>

> *Stubs* and *mocks* are [*test doubles*](https://martinfowler.com/bliki/TestDouble.html) – simplistic replacements for real\-world components\. They are used to run the business logic in isolation – without the need to deploy any heavyweight libraries or services the logic may depend upon\. A *stub* supports a single usage scenario in a single test case while a *mock* is more generic – its behavior is programmed on a per test basis\.

</aside>

### Full encapsulation: [Ports and Adapters]({{< relref "#ports-and-adapters-hexagonal-architecture" >}})

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Full%20Isolation.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Full%20Isolation.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Full%20Isolation.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Full%20Isolation.png" alt="A system with its core fully isolated by adapters from both inputs and outputs. The core depends only on its own interfaces." loading="lazy" width="843" height="483" style="width:100%"/>
</picture>
</a>
</figure>

Complete isolation of business logic – when both *driving* and *driven* communication is mediated by *adapters* – not only reaps the benefits of both approaches described above but also simplifies the main codebase because the entire business logic is written in its own terms, called *ubiquitous language* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\. It also lowers cognitive complexity which its programmers face because they don’t need to learn any external technology, and [allows for event replay to reproduce bugs](http://ithare.com/chapter-vc-modular-architecture-client-side-on-debugging-distributed-systems-deterministic-logic-and-finite-state-machines/) \(*any reproducible bug is a dead bug*\) if the business logic is deterministic\.

The pattern for full isolation of business logic is known as [*Ports and Adapters*](https://herbertograca.com/2017/09/14/ports-adapters-architecture/) which is the original [*Hexagonal Architecture*](https://alistair.cockburn.us/hexagonal-architecture)\.

## Examples

Several patterns apply the principles of *Hexagonal Architecture* to different extents:

- The patterns of the [*MVP family*]({{< relref "#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}}) add one or two layers of indirection between a GUI or web framework and the business logic\.
- Those of the [*MVC family*]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) use separate *adapters* for input and output flows\.
- [*Pedestal*]({{< relref "#pedestal" >}}) wraps each hardware component with a *driver*\.
- [*Ports and Adapters*]({{< relref "#ports-and-adapters-hexagonal-architecture" >}}) fully isolate the system’s business logic from any dependencies\.
- There are a few [related architectures with layered *cores*]({{< relref "#ddd-style-hexagonal-architecture-onion-architecture-clean-architecture" >}})\.
- [*Cell*]({{< relref "#cell-cluster-domain" >}}) isolates a group of services from the rest of the system\.


### Model\-View\-Presenter \(MVP\), Model\-View\-Adapter \(MVA\), Model\-View\-ViewModel \(MVVM\), Model 1 \(MVC1\), Document\-View

<figure>
<a href="/diagrams/Variants/4/MVP.png">
<picture>
<source srcset="/diagrams/Variants/4/MVP.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVP.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVP.png" alt="The control flow of Model-View-Presenter is a loop that starts with an OS GUI, is handled by the view, passes to the presenter, then down to the model, and all the way back to the OS." loading="lazy" width="923" height="383" style="width:100%"/>
</picture>
</a>
</figure>

*MVP*\-style patterns pass user input and output through one or more [*Presentation Layers*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}})\. Each pattern includes:

- *View* – the interface exposed to users\. In *Hexagonal Architecture*’s terms it is an *adapter* for the OS GUI framework\.
- An optional intermediate layer that translates between the *view* and *model*, beneficial for when the internal representation of the domain in the *model* diverges from the way it is presented to users by the *view*\. It is this component which differentiates the patterns, both in name and function\.
- *Model* – the whole system’s business logic and infrastructure, now independent from the method of presentation \(CLI, UI or web\)\.


*Document\-View* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] and [*Model 1*](https://stackoverflow.com/questions/796508/what-is-the-actual-difference-between-mvc-and-mvc-model2) \(*MVC1*\) skip the intermediate layer and connect the *view* directly to the *model* \(*document*\)\. These are the simplest [*Separated Presentation*](https://martinfowler.com/eaaDev/SeparatedPresentation.html) patterns for UI and web applications, respectively\.

In a [*Model\-View\-Presenter*](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-presenter) \(*MVP*\), the *presenter* \([*Supervising Controller*](https://martinfowler.com/eaaDev/SupervisingPresenter.html)\) receives input from the *view*, interprets it as a call to one of the *model*’s methods, retrieves the call’s results and shows them in the *view*, which is often completely dumb \([*Passive View*](https://martinfowler.com/eaaDev/PassiveScreen.html)\)\. A complex system may feature multiple *view\-presenter* pairs, one per UI screen\.

A [*Model\-View\-Adapter*](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html#MVA-Model-View-Adapter) \(*MVA*\) is quite similar to *MVP*, but it chooses the *adapter* on a per session basis while reusing the *view*\. For example, an unauthorized user, a normal user, and an admin would access the *model* through different *adapters* that would show them only the data and actions available with their permissions\.

A [*Model\-View\-ViewModel*](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-view_model) \(*MVVM*\) uses a stateful intermediary \(*ViewModel* or [*Presentation Model*](https://martinfowler.com/eaaDev/PresentationModel.html)\) which resembles a [*Response Cache*]({{< relref "../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Materialized View*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}), [*Reporting Database*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) or *Read Model* of [*CQRS*]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) – it stores all the data shown in the *view* in a form which is convenient for the *view* to [bind to](https://en.wikipedia.org/wiki/Data_binding)\. Changes in the *view* are propagated to the *ViewModel* which translates them into requests to the underlying application \(the true *model*\)\. Changes in the *model* \(independent or resulting from user actions\) are propagated to the *ViewModel* and, eventually, to the *view*\.

All those patterns exploit modern OS or GUI frameworks’ widgets which handle and process mouse and keyboard input, thus [removing](https://mvc.givan.se/papers/Twisting_the_Triad.pdf) the need for a separate \(input\) *controller* \(see [below]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}})\)\.

<figure>
<a href="/diagrams/Variants/4/MVP%20-%20subtypes.png">
<picture>
<source srcset="/diagrams/Variants/4/MVP%20-%20subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVP%20-%20subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVP%20-%20subtypes.png" alt="Diagrams of MVP with a view-presenter pair for each screen, MVA with different adapters for different kinds of users, MVVM with data in ViewModel, and simple Document-View and Model 1." loading="lazy" width="1284" height="903" style="width:100%"/>
</picture>
</a>
</figure>

### Model\-View\-Controller \(MVC\), Action\-Domain\-Responder \(ADR\), Resource\-Method\-Representation \(RMR\), Model 2 \(MVC2\), Game Development Engine

<figure>
<a href="/diagrams/Variants/4/MVC.png">
<picture>
<source srcset="/diagrams/Variants/4/MVC.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVC.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVC.png" alt="The control flow in Model-View-Controller starts with mouse events handled by the controller which calls the model which calls the view which updates the display." loading="lazy" width="983" height="303" style="width:100%"/>
</picture>
</a>
</figure>

When your presentation’s input and output diverge \(raw mouse movement vs 3D graphics in UI, HTTP requests vs HTML pages in websites\), it makes sense to separate the [*Presentation Layer*]({{< relref "../extension-metapatterns/proxy.md#user-interface-presentation-layer-separated-presentation-command-line-interface-cli-graphical-user-interface-gui-frontend-human-machine-interface-hmi-man-machine-interface-mmi-operator-interface" >}}) into dedicated components for input and output\.

*Model\-View\-Controller* \(*MVC*\) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] allows for cross\-platform development of hand\-crafted UI applications \(which was necessary before universal UI frameworks emerged\) by abstracting the system’s *model* \(its main logic and data, the *core* of *Hexagonal Architecture*\) from its [user interface]({{< relref "../basic-metapatterns/layers.md#interface-api-or-ui" >}}) containing platform\-specific *controller* \(input\) and *view* \(output\):

- The *controller* translates raw input into calls to the business\-centric model’s API\. It may also hide or lock widgets in the *view* when required by the input or if the *model*’s state changes\.
- The *model* is the bulk of UI\-agnostic code which executes *controller*’s requests and notifies the *view* and, optionally, *controller* when its data changes\.
- Upon receiving a notification, the *view* reads, transforms, and presents to the user the subset of the *model*’s data which it covers\.


Each widget on the screen [may have](https://martinfowler.com/eaaDev/uiArchs.html#ModelViewController) its own *model\-view* pair\. The absence of an intermediate layer between the *view* and *model* makes the *view* heavyweight as it has to translate the *model*'s data format into something presentable to users – the flaw addressed by the 3\-layered [*MVP* patterns discussed above]({{< relref "#model-view-presenter-mvp-model-view-adapter-mva-model-view-viewmodel-mvvm-model-1-mvc1-document-view" >}})\.

Both [*Action\-Domain\-Responder*](https://github.com/pmjones/adr#action-domain-responder) \(*ADR*\) and [*Resource\-Method\-Representation*](https://herbertograca.com/2018/08/31/resource-method-representation/) \(*RMR*\) are web layer patterns\. An *action* \(*method*\) receives a request, calls into a *domain* \(*resource*\) to make changes and retrieve data and brings the results to a *responder* \(*representation*\) which prepares the return message or web page\. *ADR* is technology\-agnostic while *RMR* is HTTP\-centric\.

[*Model 2*](https://stackoverflow.com/questions/796508/what-is-the-actual-difference-between-mvc-and-mvc-model2) \(*MVC2*\) is a similar pattern from the Java world with integration logic [implemented](https://github.com/pmjones/adr/blob/master/MVC-MODEL-2.md) in the *controller*\.

A [*game development engine*](https://slideplayer.com/slide/12426213/) creates a higher\-level abstraction over input from mouse / keyboard / joystick and output to sound card / GPU while more powerful engines may also [model physics](https://discussions.unity.com/t/unity3d-architecture/565787) and character interactions\. The role is quite similar to what the original *MVC* did, with a couple of differences:

- Games often have to deal with the low\-level and very chatty interfaces of hardware components, thus the input and output are at the bottom side of the system diagram\.
- The framework itself makes a cohesive layer, becoming a kind of [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\.


Another difference is that while *MVC* provides for changing target platforms by rewriting its minor components \(*view* and *controller*\), you are very unlikely to change your game framework – instead, it is the framework itself that makes all the platforms look identical to your code\.

<figure>
<a href="/diagrams/Variants/4/MVC%20-%20subtypes.png">
<picture>
<source srcset="/diagrams/Variants/4/MVC%20-%20subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVC%20-%20subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVC%20-%20subtypes.png" alt="Diagrams of MVC with a dedicated view-controller pair for each widget, ADR and RMR where the action calls the responder, Model 2 with an orchestrating controller, and a game development engine." loading="lazy" width="1303" height="824" style="width:100%"/>
</picture>
</a>
</figure>

### [Pedestal]({{< relref "../basic-metapatterns/services.md#inexact-device-drivers-pedestal" >}})

<figure>
<a href="/diagrams/Variants/4/Pedestal.png">
<picture>
<source srcset="/diagrams/Variants/4/Pedestal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Pedestal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Pedestal.png" alt="A Pedestal has a control layer mediating hardware drivers which wrap each hardware component. An operating system adds a kernel between the drivers and an application that uses them." loading="lazy" width="1303" height="384" style="width:100%"/>
</picture>
</a>
</figure>

[*Pedestal*](https://alistair.cockburn.us/hexagonal-architecture) is named after the shape of its diagram and describes a system that operates multiple hardware components\. On one hand, *software* takes years to write and stabilize\. On the other hand, there are many versions and even vendors for each kind of *hardware* component, and they change over years\. Therefore, the bulk of the *software* that contains the system logic \(called [*control*]({{< relref "../extension-metapatterns/orchestrator.md" >}})\) is decoupled from the *hardware* it manages through the use of *hardware*\-specific [*drivers*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}})\. As a result, changes in *hardware* require updates only to matching *drivers*, the business logic remaining intact\.

A similar pattern is in use with [*OS kernels*]({{< relref "../implementation-metapatterns/microkernel.md#operating-system" >}}), adjusted for the fact that the business logic now resides in user applications, the *kernel* acting as an [extra layer of indirection](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering)\.

### [Ports and Adapters]({{< relref "#full-encapsulation-ports-and-adapters" >}}), Hexagonal Architecture

<figure>
<a href="/diagrams/Variants/4/Monolithic%20Hexagonal.png">
<picture>
<source srcset="/diagrams/Variants/4/Monolithic%20Hexagonal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Monolithic%20Hexagonal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Monolithic%20Hexagonal.png" alt="Adapters of the Hexagonal Architecture translate between the interfaces of its core and those of the adapted external components." loading="lazy" width="904" height="483" style="width:100%"/>
</picture>
</a>
</figure>

[*Ports and Adapters*](https://alistair.cockburn.us/hexagonal-architecture/) – the original *Hexagonal Architecture* – provides full isolation for its business logic \(*core*\) by injecting *adapters* in all its [communication pathways]({{< relref "#variants-by-encapsulation" >}})\. As a result, the *core* depends only on interfaces, called *ports* \(hence the name of the pattern\), which it defines, with the benefits discussed [earlier in this chapter]({{< relref "#full-encapsulation-ports-and-adapters" >}})\.

Just like [*MVC*]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) it is based on, *Ports and Adapters* does not care about the contents or structure of its *core* – it is all about isolating the *core* from its environment\. The *core* may have [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) or [*Modules*]({{< relref "../basic-metapatterns/services.md#synchronous-modules-modular-monolith-modulith" >}}) or even [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) inside, but the pattern has nothing to say about them\.

### DDD\-Style Hexagonal Architecture, Onion Architecture, Clean Architecture

<figure>
<a href="/diagrams/Variants/4/Layered%20Hexagonal.png">
<picture>
<source srcset="/diagrams/Variants/4/Layered%20Hexagonal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Layered%20Hexagonal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Layered%20Hexagonal.png" alt="Control flows for changing an entity in puristic Domain-Driven Design, pragmatic Domain-Driven Design, and Onion Architecture." loading="lazy" width="1123" height="563" style="width:100%"/>
</picture>
</a>
</figure>

As *Hexagonal Architecture* built upon the [*Domain\-Driven Design*](https://en.wikipedia.org/wiki/Domain-driven_design)’s \(*DDD*\) idea of isolating business logic with [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(see the [*Anticorruption Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) and [*Open Host Service*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) patterns\), it was quickly integrated back into DDD \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\]\. However, as *Ports and Adapters* appeared later than the [original DDD book]({{< relref "../appendices/books-referenced.md#ddd" >}}), there is no universal agreement on how this architecture should work:

- The cleanest way is for the [*domain*]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) layer to have nothing to do with the database – with this approach the [*application*]({{< relref "../basic-metapatterns/layers.md#application-use-cases-or-integration" >}}) asks the [*repository*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) \(the database *adapter*\) to create *aggregates* \(domain objects\), then executes its business actions on the aggregates and tells the repository to save the changed aggregates back to the database\.
- Others say that in practice the logic inside an aggregate may have to read additional information from the database or even depend on the result of persisting parts of the aggregate\. Thus it is the aggregate, not the *application*, which should save its changes, and the logic of accessing the database leaks into the domain layer\.
- [*Onion Architecture*](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/) \(see the [original article](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/) for the meaning of the name\), one of early developments of *Hexagonal Architecture* and DDD, always splits the domain layer into a *domain model* and a *domain services*\. The *domain model* layer contains classes with business data and business logic, which are loaded and saved by the *domain services* layer just above it\. And the upper *application services* layer drives use cases by calling into both domain services and the *domain model*\.
- There is also [*Clean Architecture*](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) which seems to generalize the approaches above without delving into practical details – thus the way it saves its aggregates remains a mystery\.


### Cell, Cluster, Domain

<figure>
<a href="/diagrams/Variants/4/Cell.png">
<picture>
<source srcset="/diagrams/Variants/4/Cell.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Cell.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Cell.png" alt="Several intercommunicating subservices are wrapped with a cell gateway that receives client requests, adapters for outgoing communication, and a plugin." loading="lazy" width="1183" height="424" style="width:100%"/>
</picture>
</a>
</figure>

A [*Cell*](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) \(WSO2 name\), *Cluster* \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\], or [*Domain*](https://www.uber.com/en-UA/blog/microservice-architecture/) \(Uber’s name\) is an encapsulated cluster of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) which implements a subdomain and is usually deployed as a single unit\. It is modeled after a living cell whose contents are isolated with a membrane\. [As a rule](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography) \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\], the communication inside a *Cell* is synchronous, allowing for complex [*orchestrated*]({{< relref "../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}) use cases that involve tightly coupled *Cell* components\. Contrariwise, *Cells* are usually loosely coupled and the communications between them tend to be asynchronous \([*choreography*]({{< relref "../foundations-of-software-architecture/arranging-communication/choreography.md" >}})\)\.

 A *Cell* may naturally emerge when a [*subdomain service*]({{< relref "../basic-metapatterns/services.md#whole-subdomain-sub-domain-services-macroservices" >}}) becomes too large for comfortable development, which usually means that at least its [*domain* layer]({{< relref "../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) \(already limited to a single subdomain\) is to be subdivided into sub\-subdomain components\. If other layers remain intact, this leads to a [*Sandwich*]({{< relref "../extension-metapatterns/sandwich.md" >}}), otherwise the result is a subsystem of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) or a [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\. 

<figure>
<a href="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Subtypes.png">
<picture>
<source srcset="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Subtypes.png" alt="Diagrams for: a Cell with a Sandwich, a Cell with services, and a Cell with a pipeline." loading="lazy" width="883" height="384" style="width:88%"/>
</picture>
</a>
</figure>

Another way a *Cell* can arise is when architects have overcommitted themselves to [*Microservices*]({{< relref "../basic-metapatterns/services.md#part-of-a-subdomain-microservices" >}}), gradually [introducing hundreds of them](https://www.uber.com/en-UA/blog/microservice-architecture/) and turning their project into a [*Microservice Hell*](https://www.reddit.com/r/programming/comments/10xvltx/microservice_hell/)\. Now they need to group their services to:

- Have a clear high\-level picture of what is going on in the system\.
- Cut accidental dependencies between their services and the teams behind them\.
- Improve latency by co\-locating the services that interact intensely\.


<figure>
<a href="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Evolutions.png">
<picture>
<source srcset="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Evolutions.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Evolutions.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Cell%20-%20Basic%20-%20Evolutions.png" alt="A layered system transforms into a Sandwich-based Cell. A group of stand-alone services is aggregated into a cell behind a Cell gateway." loading="lazy" width="1283" height="343" style="width:100%"/>
</picture>
</a>
</figure>

In the simplest implementation a *Cell*’s contents are hidden from the *Cell*’s clients by a [*Cell Gateway*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) which acts as an [*Open Host Service*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}), allowing for anything inside the *Cell* to be changed at will with no effect on the outside world\.

<aside>

> In practice there are three kinds of outgoing traffic: responses to incoming client requests, pub/sub notifications, and requests to external services\. In a *Cell*, *responses* are sure to pass through or be generated by the *Cell Gateway*\. Indeed, a response usually reuses the request’s transport, therefore if a request arrives at the *Gateway*, the corresponding response should also start there\. *Notifications* are harder to pinpoint: on one hand, they are a part of the *Cell*’s API which the *Gateway* takes care of\. However, that means that the *Cell*’s internals must be aware of the *Gateway*’s existence to use it for notifications, thus creating a dependency that violates the [normal order for a *layered system*]({{< relref "../basic-metapatterns/layers.md#dependencies" >}})\. Finally, *outgoing requests* bypass the *Cell Gateway* whose role is limited to the *Cell’*s API\.

</aside>

Better developed *Cells* employ [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) for outgoing requests to build an [*Anticorruption Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-abstraction-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-database-access-layer-data-mapper-repository-driver" >}}) that protects the *Cell*’s contents from changes in its environment\. Please note that, unlike in [*Ports and Adapters*]({{< relref "#ports-and-adapters-hexagonal-architecture" >}}), there is no *Adapter* for a *Cell*’s database\(s\) as it is inside the *Cell*’s perimeter\.

Another improvement, popularized by Uber’s [*Domains*](https://www.uber.com/en-UA/blog/microservice-architecture/), is the use of [*Ambassador Plugins*]({{< relref "../implementation-metapatterns/plugins.md#ambassador-plugin-logic-extension" >}}) which run other services’ business logic inside your *Cell*\. That both avoids slow intercell calls and boosts the system’s fault tolerance as each *Cell* can now operate independently\.

<figure>
<a href="/diagrams/Variants/4/Cell%20-%20Full-Featured%20-%20Plugins.png">
<picture>
<source srcset="/diagrams/Variants/4/Cell%20-%20Full-Featured%20-%20Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Cell%20-%20Full-Featured%20-%20Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Cell%20-%20Full-Featured%20-%20Plugins.png" alt="Injecting a part of a Cell's business logic into another Cell as an ambassador plugin improves performance by avoiding expensive intercell calls." loading="lazy" width="1063" height="284" style="width:100%"/>
</picture>
</a>
</figure>

<aside>

> [In Uber](https://www.uber.com/en-UA/blog/microservice-architecture/) the service responsible for a driver’s status accepts *Plugins* from other services, such as safety checks or compliance, which can block the driver from appearing in the system and responding to ride requests\.

</aside>

*Cells* facilitate recursive decomposition by subdomain\. They are the building blocks for the following patterns:

- [*Cell\-Based Architecture*]({{< relref "../fragmented-metapatterns/hierarchy.md#in-depth-hierarchy-cell-based-microservice-architecture-wso2-version-segmented-microservice-architecture-services-of-services-clusters-of-services" >}}) which is [*hierarchical*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) [*Services*]({{< relref "../basic-metapatterns/services.md" >}})\.
- [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}}) which is a [*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) boosted by [*Ambassador Plugins*]({{< relref "../implementation-metapatterns/plugins.md#ambassador-plugin-logic-extension" >}})\.


## Summary

*Hexagonal Architecture* isolates a component’s business logic from its external dependencies by inserting *adapters* between them\. It protects from *vendor lock\-in* and allows for late changes of third\-party components but requires all the APIs to be designed before programming can start and often hinders performance optimizations\.