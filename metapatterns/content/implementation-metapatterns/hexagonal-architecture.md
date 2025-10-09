+++
weight = 6
title = "Hexagonal Architecture"
description = "Hexagonal Architecture isolates a component's business logic from its environment by hiding all the component's dependencies behind Adapters."
images = ["/diagrams/Main/Hexagonal%20Architecture.png"]
[sitemap]
  priority = 0.8
+++

# Hexagonal Architecture

<figure>
<a href="/diagrams/Main/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Main/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Main/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Main/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="982" height="602" style="width:100%"/>
</picture>
</a>
</figure>

*Trust no one\.* Protect your code from external dependencies\.

<ins>Known as:</ins> Hexagonal Architecture, or originally as Ports and Adapters\.

<ins>Variants:</ins> 

By placement of adapters:

- Adapters on the external component’s side\.
- Adapters on the *core* side\.


Examples – [Hexagonal Architecture](https://herbertograca.com/2017/09/14/ports-adapters-architecture/):

- Hexagonal Architecture / [Ports and Adapters](https://alistair.cockburn.us/hexagonal-architecture/),
- DDD\-Style Hexagonal Architecture \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\] / [Onion Architecture](https://herbertograca.com/2017/09/21/onion-architecture/) / [Clean Architecture](https://herbertograca.com/2017/09/28/clean-architecture-standing-on-the-shoulders-of-giants/)\.


Examples – [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html):

- \(*Layered*\) [Model\-View\-Presenter](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-presenter) \(MVP\), [Model\-View\-Adapter](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html#MVA-Model-View-Adapter) \(MVA\), [Model\-View\-ViewModel](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-view_model) \(MVVM\), [Model 1](https://lostechies.com/derekgreer/2007/08/25/interactive-application-architecture/) \(MVC1\), Document\-View \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\],
- \(*Pipelined*\) Model\-View\-Controller \(MVC\) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] / [Action\-Domain\-Responder](https://github.com/pmjones/adr#action-domain-responder) \(ADR\) / [Resource\-Method\-Representation](https://herbertograca.com/2018/08/31/resource-method-representation/) \(RMR\) / [Model 2](https://lostechies.com/derekgreer/2007/08/25/interactive-application-architecture/) \(MVC2\) / [Game Development Engine](https://slideplayer.com/slide/12426213/)\.


<ins>Structure:</ins> A monolithic business logic extended with a set of \(adapter, component\) pairs that encapsulate external dependencies\.

<ins>Type:</ins> Implementation\.

| *Benefits* | *Drawbacks* |
| --- | --- |
| Isolates business logic from external dependencies | Suboptimal performance |
| Facilitates the use of stubs/mocks for testing and development | The vendor\-independent interfaces must be designed before the start of development |
| Allows for qualities to vary between the external components and the business logic |  |
| The programmers of business logic don’t need to learn any external technologies |  |

<ins>References:</ins> [Herberto Graça’s chronicles](https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/) is the main collection of patterns from this chapter\. *Hexagonal Architecture* has [the original article](https://alistair.cockburn.us/hexagonal-architecture/) and a brief summary of its layered variant in \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\]\. Most of the *Separated Presentation* patterns are featured on Wikipedia and there are collections of them from [Martin Fowler](https://martinfowler.com/eaaDev/uiArchs.html), [Anthony Ferrara](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html) and [Derek Greer](https://lostechies.com/derekgreer/2007/08/25/interactive-application-architecture/)\.

*Hexagonal Architecture* is a variation of [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}}) that aims for total self\-sufficiency of business logic\. Any third\-party tools, whether libraries, services or databases, are hidden behind [*adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[GoF]({{< relref "../appendices/books-referenced.md#gof" >}})\] that translate the external module’s interface into a *service provider interface* \(*SPI*\) defined by the *core* module and called *port*\. The core’s business logic depends only on the ports that its developers defined – a perfect use of [*dependency inversion*](https://en.wikipedia.org/wiki/Dependency_inversion_principle) – and manipulates interfaces that were designed in the most convenient way\. The benefits of this architecture include the core’s cross\-platform nature, easy development and testing with [stubs or mocks](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub), support for event replay and protection from [vendor lock\-in](https://en.wikipedia.org/wiki/Vendor_lock-in)\. It also allows for replacement of any external library at late stages of the project\. The flexibility is paid for with a somewhat longer system design stage and lost optimization opportunities\. There is also a high risk to design a leaky abstraction – an SPI that looks generic but whose contract matches that of the component it encapsulates, making it much harder than expected to change the component’s vendor\.

<aside>

> *Stubs* and *mocks* are [*test doubles*](https://martinfowler.com/bliki/TestDouble.html) – simplistic replacements for real\-world components\. They are used to run the business logic in isolation – without the need to deploy any heavyweight libraries or services the logic may depend upon\. A *stub* supports a single usage scenario in a single test case while a *mock* is more generic – its behavior is programmed on a per test basis\.

</aside>

### Performance

*Hexagonal Architecture* is a strange beast performance\-wise\. The generic interfaces \(ports\) between the core and adapters stand in the way of whole\-system optimization and may add context switching\. Still, at the same time, each adapter concentrates all the vendor\-specific code for its external dependency, which makes the adapter a perfect single place for aggressive optimization by an expert or consultant who is proficient with the adapted third\-party software but does not have time to learn the details of your business logic\. Thus, some opportunities for optimization are lost while others emerge\.

In rare cases the system may benefit from direct communication between the adapters\. However, that requires several of them to be compatible or polymorphic, in which case your *Hexagonal Architecture* may in fact be a kind of shallow [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\. Examples include a service that uses several databases which are kept in sync through [*Change Data Capture*](https://www.dremio.com/wiki/change-data-capture/) \(*CDC*\) or a telephony gateway that interconnects various kinds of voice devices\.

<figure>
<a href="/diagrams/Performance/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Performance/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Performance/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Performance/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="703" height="401" style="width:90%"/>
</picture>
</a>
</figure>

### Dependencies

Each [adapter]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) breaks the dependency between the core that contains business logic and an adapted component\. This makes all the system’s components mutually independent – and easily interchangeable and evolvable – except for the adapters themselves, which are small enough to be rewritten as need arises\.

<figure>
<a href="/diagrams/Dependencies/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Dependencies/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Dependencies/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Dependencies/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="803" height="483" style="width:84%"/>
</picture>
</a>
</figure>

### Applicability

*Hexagonal Architecture* <ins>benefits</ins>:

- *Medium\-sized or larger components\.* The programmers don’t need to learn details of external technologies and may concentrate on the business logic instead\. The code of the *core* becomes smaller as all the details of managing external components are moved into their *adapters*\.
- *Cross\-platform development\.* The core is naturally cross\-platform as it does not depend on any \(platform\-specific\) libraries\.
- *Long\-lived products*\. Technologies come and go, your product remains\. Always be ready to change the technologies it uses\.
- *Unfamiliar domain\.* You don’t know how much load you’ll need your database to support\. You don’t know if the library you selected is stable enough for your needs\. Be prepared to replace vendors even after the public release of your product\.
- *Automated testing\.* [Stubs and mocks](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) are great for reducing load on test servers\. And stubs for the SPIs which you wrote yourself are easy as a pie\.
- *Zero bug tolerance\.* SPIs allow for event replay\. If your business logic is deterministic, you can [reproduce your user’s bugs in your office](http://ithare.com/chapter-vc-modular-architecture-client-side-on-debugging-distributed-systems-deterministic-logic-and-finite-state-machines/)\.


*Hexagonal Architecture* <ins>is not good</ins> for:

- *Small components\.* If there is little business logic, there is not much to protect, while the overhead of defining SPIs and writing adapters is high compared to the total development time\.
- *Write\-and\-forget projects\.* You don’t want to waste your time on long\-term survivability of your code\.
- *Quick start*\. You need to show the results right now\. No time for good architecture\.
- *Low latency*\. The adapters slow down communication\. This is somewhat alleviated by creating direct communication channels between the adapters to bypass the core\.


### Relations

<figure>
<a href="/diagrams/Relations/Hexagonal%20Architecture.png">
<picture>
<source srcset="/diagrams/Relations/Hexagonal%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Relations/Hexagonal%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Relations/Hexagonal%20Architecture.png" alt="Hexagonal Architecture" loading="lazy" width="1023" height="602" style="width:100%"/>
</picture>
</a>
</figure>

*Hexagonal Architecture*:

- Is a kind of [*Plugins*]({{< relref "../implementation-metapatterns/plugins.md" >}})\.
- May be a shallow [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})\.
- Implements [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}) or [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Extends [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}}), [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}}) or, rarely, [*Services*]({{< relref "../basic-metapatterns/services.md" >}}) with one or two layers of *services*\.
- The [*MVC* family of patterns]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) is also [derived]({{< relref "../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md" >}}) from [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.


## Variants by placement of adapters

One possible variation in a distributed or asynchronous *Hexagonal Architecture* is the deployment of [*adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), which may reside adjacent to the core or with the components they adapt:

### Adapters on the external component side

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20Components.png" alt="Hexagonal - Adapters with Components" loading="lazy" width="823" height="483" style="width:93%"/>
</picture>
</a>
</figure>

If your team owns the component adapted, the *adapter* may be placed next to it\. That usually makes sense because a single domain message \(in the terms of your business logic\) tends to unroll into a series of calls to an external component\. The fewer messages you send, the faster your system is\.

This resembles [*Sidecar*](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#heading=h.d9s7thhgp6q9) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] and [*Open Host Service*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\.

### Adapters on the core side

<figure>
<a href="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.png">
<picture>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Hexagonal%20-%20Adapters%20with%20the%20Core.png" alt="Hexagonal - Adapters with the Core" loading="lazy" width="1063" height="324" style="width:100%"/>
</picture>
</a>
</figure>

Sometimes you need to adapt an external service which you don’t control\. In that case the only real option is to place its *adapter* together with your *core* logic\. In theory, the adapter can be deployed as a separate component, maybe in a [*Sidecar*]({{< relref "../extension-metapatterns/proxy.md#on-the-system-side-sidecar" >}}) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\], but that may slow down communication\.

This approach resembles [*Ambassador*](https://docs.google.com/document/d/1hzBn-RzzNDcArAWcvXaXgw2nl6O_ryDKE51Xve18zOs/edit?pli=1&tab=t.0#heading=h.ko68gfp4bjq0) \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}})\] and [*Anticorruption Layer*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\]\.

## Examples – Hexagonal Architecture

*Hexagonal Architecture* protects business logic from all its dependencies\. It is simple and unambiguous\. It does not come in many shapes:

### Hexagonal Architecture, Ports and Adapters

<figure>
<a href="/diagrams/Variants/4/Monolithic%20Hexagonal.png">
<picture>
<source srcset="/diagrams/Variants/4/Monolithic%20Hexagonal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Monolithic%20Hexagonal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Monolithic%20Hexagonal.png" alt="Monolithic Hexagonal" loading="lazy" width="863" height="363" style="width:100%"/>
</picture>
</a>
</figure>

Just like [*MVC*]({{< relref "#model-view-controller-mvc-action-domain-responder-adr-resource-method-representation-rmr-model-2-mvc2-game-development-engine" >}}) it is based on, the original *Hexagonal Architecture* \([*Ports and Adapters*](https://alistair.cockburn.us/hexagonal-architecture/)\) does not care about the contents or structure of its *core* – it is all about isolating the core from the environment\. The core may have layers or modules or even plugins inside, but the pattern has nothing to say about them\.

### DDD\-Style Hexagonal Architecture, Onion Architecture, Clean Architecture

<figure>
<a href="/diagrams/Variants/4/Layered%20Hexagonal.png">
<picture>
<source srcset="/diagrams/Variants/4/Layered%20Hexagonal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/Layered%20Hexagonal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/Layered%20Hexagonal.png" alt="Layered Hexagonal" loading="lazy" width="1011" height="543" style="width:100%"/>
</picture>
</a>
</figure>

As *Hexagonal Architecture* built upon the [DDD](https://en.wikipedia.org/wiki/Domain-driven_design)’s idea of isolating business logic with [*Adapters*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}), it was quickly integrated back into DDD \[[LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}})\]\. However, as *Ports and Adapters* appeared later than the [original DDD book]({{< relref "../appendices/books-referenced.md#ddd" >}}), there is no universal agreement on how the thing should work:

- The cleanest way is for the *domain* layer to have nothing to do with the database – with this approach the *application* asks the [*repository*]({{< relref "../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \(the database *adapter*\) to create *aggregates* \(domain objects\), then executes its business actions on the aggregates and tells the repository to save the changed aggregates back to the database\.
- Others say that in practice the logic inside an aggregate may have to read additional information from the database or even depend on the result of persisting parts of the aggregate\. Thus it is the aggregate, not the application, which should save its changes, and the logic of accessing the database leaks into the domain layer\.
- [*Onion Architecture*](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/), one of early developments of *Hexagonal Architecture* and DDD, always splits the domain layer into a *domain model* and a *domain services*\. The *domain model* layer contains classes with business data and business logic, which are loaded and saved by the *domain services* layer just above it\. And the upper *application services* layer drives use cases by calling into both domain services and domain model\.
- There is also [*Clean Architecture*](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) which seems to generalize the approaches above without delving into practical details – thus the way it saves its aggregates remains a mystery\.


## Examples – Separated Presentation

[*Separated Presentation*](https://martinfowler.com/eaaDev/SeparatedPresentation.html) protects business logic from a dependency on *presentation* \(interactions with the system’s user via a window, command line, or web page\)\. There is a [great variety](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html) of such patterns, commonly known as *Model\-View\-Controller* \(MVC\) alternatives\. They are derived from *Hexagonal Architecture* by omitting every component not directly involved in user interactions and make three structurally distinct groups:

- Bidirectional flow – the *view* \(user\-facing component\) both receives input and produces output and there is often an explicit *adapter* between it and the main system, resulting in [*Layers*]({{< relref "../basic-metapatterns/layers.md" >}})\.
- Unidirectional flow – the *controller* receives input while the *view* produces output, [forming]({{< relref "../analytics/comparison-of-architectural-patterns/pipelines-in-architectural-patterns.md#model-view-controller-mvc" >}}) a kind of [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\.
- Hierarchical with multiple *models*, [discussed]({{< relref "../fragmented-metapatterns/hierarchy.md#top-down-hierarchy-orchestrator-of-orchestrators-presentation-abstraction-control-pac-hierarchical-model-view-controller-hmvc" >}}) in the [*Hierarchy*]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) chapter\.


All of them aim at making the business logic presentation\-agnostic \(thus cross\-platform and developed by an independent team\), but differ in their complexity, flexibility and best use cases\.

### Model\-View\-Presenter \(MVP\), Model\-View\-Adapter \(MVA\), Model\-View\-ViewModel \(MVVM\), Model 1 \(MVC1\), Document\-View

<figure>
<a href="/diagrams/Variants/4/MVP.png">
<picture>
<source srcset="/diagrams/Variants/4/MVP.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVP.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVP.png" alt="MVP" loading="lazy" width="923" height="363" style="width:100%"/>
</picture>
</a>
</figure>

*MVP*\-style patterns pass user input and output through one or more presentation [*layers*]({{< relref "../basic-metapatterns/layers.md" >}})\. Each pattern includes:

- *View* – the interface exposed to users\.
- An optional intermediate layer that translates between the *view* and *model*\. It is the component which differentiates the patterns, both in name and function\.
- *Model* – the whole system’s business logic and infrastructure, now independent from the method of presentation \(CLI, UI or web\)\.


*Document\-View* \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}})\] and [*Model 1*](https://stackoverflow.com/questions/796508/what-is-the-actual-difference-between-mvc-and-mvc-model2) \(*MVC1*\) skip the intermediate layer and connect the *view* directly to the *model* \(*document*\)\. These are the simplest [*Separated Presentation*](https://martinfowler.com/eaaDev/SeparatedPresentation.html) patterns for UI and web applications, correspondingly\.

In a [*Model\-View\-Presenter*](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-presenter) \(*MVP*\), the *presenter* \([*Supervising Controller*](https://martinfowler.com/eaaDev/SupervisingPresenter.html)\) receives input from the *view*, interprets it as a call to one of the model’s methods, retrieves the call’s results and shows them in the view, which is often completely dumb \([*Passive View*](https://martinfowler.com/eaaDev/PassiveScreen.html)\)\. A complex system may feature multiple view\-presenter pairs, one per UI screen\.

A [*Model\-View\-Adapter*](https://blog.ircmaxell.com/2014/11/alternatives-to-mvc.html#MVA-Model-View-Adapter) \(*MVA*\) is quite similar to *MVP*, but it chooses the adapter on a per session basis while reusing the view\. For example, an unauthorized user, a normal user, and an admin would access the model through different adapters that would show them only the data and actions available with their permissions\.

A [*Model\-View\-ViewModel*](https://herbertograca.com/2017/08/17/mvc-and-its-variants/#model-view-view_model) \(*MVVM*\) uses a stateful intermediary \(*ViewModel* or [*Presentation Model*](https://martinfowler.com/eaaDev/PresentationModel.html)\) which resembles a [*Response Cache*]({{< relref "../extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}}), [*Materialized View*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#memory-image-materialized-view" >}}), [*Reporting Database*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) or *Read Model* of [*CQRS*]({{< relref "../fragmented-metapatterns/layered-services.md#command-query-responsibility-segregation-cqrs" >}}) – it stores all the data shown in the view in a form which is convenient for the view to [bind to](https://en.wikipedia.org/wiki/Data_binding)\. Changes in the view are propagated to the ViewModel which translates them into requests to the underlying application \(the true model\)\. Changes in the model \(independent or resulting from user actions\) are propagated to the ViewModel and, eventually, to the view\.

All those patterns exploit modern OS or GUI frameworks’ widgets which handle and process mouse and keyboard input, thus [removing](https://mvc.givan.se/papers/Twisting_the_Triad.pdf) the need for a separate \(input\) *controller* \(see below\)\.

<figure>
<a href="/diagrams/Variants/4/MVP%20-%20subtypes.png">
<picture>
<source srcset="/diagrams/Variants/4/MVP%20-%20subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVP%20-%20subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVP%20-%20subtypes.png" alt="MVP - subtypes" loading="lazy" width="1284" height="903" style="width:100%"/>
</picture>
</a>
</figure>

### Model\-View\-Controller \(MVC\), Action\-Domain\-Responder \(ADR\), Resource\-Method\-Representation \(RMR\), Model 2 \(MVC2\), Game Development Engine

<figure>
<a href="/diagrams/Variants/4/MVC.png">
<picture>
<source srcset="/diagrams/Variants/4/MVC.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVC.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVC.png" alt="MVC" loading="lazy" width="943" height="303" style="width:100%"/>
</picture>
</a>
</figure>

When your presentation’s input and output diverge \(raw mouse movement vs 3D graphics in UI, HTTP requests vs HTML pages in websites\), it makes sense to separate the presentation layer into dedicated components for input and output\.

*Model\-View\-Controller* \(*MVC*\) \[[POSA1]({{< relref "../appendices/books-referenced.md#posa1" >}}), [POSA4]({{< relref "../appendices/books-referenced.md#posa4" >}})\] allows for cross\-platform development of hand\-crafted UI applications \(which was necessary before universal UI frameworks emerged\) by abstracting the system’s *model* \(its main logic and data, the *core* of *Hexagonal Architecture*\) from its user interface containing platform\-specific *controller* \(input\) and *view* \(output\):

- The *controller* translates raw input into calls to the business\-centric model’s API\. It may also hide or lock widgets in the view when the model’s state changes\.
- The *model* is the main UI\-agnostic application which executes controller’s requests and notifies the view and, optionally, controller when its data changes\.
- Upon receiving a notification, the *view* reads, transforms, and presents to the user the subset of the model’s data which it covers\.


Each widget on the screen [may have](https://martinfowler.com/eaaDev/uiArchs.html#ModelViewController) its own model\-view pair\. The absence of an intermediate layer between the view and model makes the view heavyweight as it has to translate the model's data format into something presentable to users – the flaw addressed by the *MVP* \(3\-layered\) patterns discussed above\.

Both [*Action\-Domain\-Responder*](https://github.com/pmjones/adr#action-domain-responder) \(*ADR*\) and [*Resource\-Method\-Representation*](https://herbertograca.com/2018/08/31/resource-method-representation/) \(*RMR*\) are web layer patterns\. An *action* \(*method*\) receives a request, calls into a *domain* \(*resource*\) to make changes and retrieve data and brings the results to a *responder* \(*representation*\) which prepares the return message or web page\. *ADR* is technology\-agnostic while *RMR* is HTTP\-centric\.

[*Model 2*](https://stackoverflow.com/questions/796508/what-is-the-actual-difference-between-mvc-and-mvc-model2) \(*MVC2*\) is a similar pattern from the Java world with integration logic [implemented](https://github.com/pmjones/adr/blob/master/MVC-MODEL-2.md) in the controller\.

A [*game development engine*](https://slideplayer.com/slide/12426213/) creates a higher\-level abstraction over input from mouse / keyboard / joystick and output to sound card / GPU while more powerful engines may also [model physics](https://discussions.unity.com/t/unity3d-architecture/565787) and character interactions\. The role is quite similar to what the original *MVC* did, with a couple of differences:

- Games often have to deal with the low\-level and very chatty interfaces of hardware components, thus the input and output are at the bottom side of the system diagram\.
- The framework itself makes a cohesive layer, becoming a kind of [*Microkernel*]({{< relref "../implementation-metapatterns/microkernel.md" >}})\.


Another difference is that while *MVC* provides for changing target platforms by rewriting its minor components \(*view* and *controller*\), you are very unlikely to change your game framework – instead, it is the framework itself that makes all the platforms look identical to your code\.

<figure>
<a href="/diagrams/Variants/4/MVC%20-%20subtypes.png">
<picture>
<source srcset="/diagrams/Variants/4/MVC%20-%20subtypes.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Variants/4/MVC%20-%20subtypes.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Variants/4/MVC%20-%20subtypes.png" alt="MVC - subtypes" loading="lazy" width="1303" height="824" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

*Hexagonal Architecture* isolates a component’s business logic from its external dependencies by inserting *adapters* between them\. It protects from *vendor lock\-in* and allows for late changes of third\-party components but requires all the APIs to be designed before programming can start and often hinders performance optimizations\.