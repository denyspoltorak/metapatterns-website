+++
weight = 4
title = "Evolutions of a Monolith that rely on Plugins"
description = "Plugins or Interpreter make a monolithic component customizable. Hexagonal Architecture protects its business logic from unstable external dependencies. "
+++

# Evolutions of a Monolith that rely on Plugins

The last group of evolutions which we review does not really change the monolithic nature of the application\. Instead, its goal is to improve the *customizability* of the [*Monolith*]({{< relref "../../part-2--basic-metapatterns/monolith.md" >}}):

- Vanilla [*Plugins*]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}}) is the most direct approach which relies on replaceable bits of logic\.
- [*Hexagonal Architecture*]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) is a subtype of *Plugins* which is all about isolating the main code from any third\-party components it uses\.
- [*Scripts*]({{< relref "../../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) is a kind of [*Microkernel*]({{< relref "../../part-5--implementation-metapatterns/microkernel.md" >}}) – yet another subtype of *Plugins* – which gives users of the system full control over its behavior\.


## Support plugins

<figure style="text-align:center">
<a href="/Evolutions/Monolith/Monolith%20to%20Plugins.png" style="outline:none">
<img src="/Evolutions/Monolith/Monolith%20to%20Plugins.png" alt="Monolith to Plugins" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Plugins]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}})\.

<ins>Goal</ins>: simplify the customization of the application’s behavior\.

<ins>Prerequisite</ins>: several aspects need to vary from customer to customer\.

*Plugins* create points of access to the system that allow engineers to collect data and govern select aspects of the system’s behavior without having to learn the system’s implementation\.

<ins>Pros</ins>: 

- The system can be modified by internal and external programmers who don’t know its internal details\.
- Customized versions become much easier to release and support\.


<ins>Cons</ins>: 

- Extensive changes may be required to expose the tunable aspects of the system\.
- Testability becomes poor because of the number of possible variants\.
- Performance is likely to degrade\.


## Isolate dependencies with Hexagonal Architecture

<figure style="text-align:center">
<a href="/Evolutions/Monolith/Monolith%20to%20Hexagonal.png" style="outline:none">
<img src="/Evolutions/Monolith/Monolith%20to%20Hexagonal.png" alt="Monolith to Hexagonal" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Hexagonal Architecture]({{< relref "../../part-5--implementation-metapatterns/hexagonal-architecture.md" >}}) \([Plugins]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}})\)\.

<ins>Goal</ins>: isolate the business logic from external dependencies\.

<ins>Prerequisite</ins>: there are third\-party or unstable components in the system\.

The main business logic will communicate with all the external components through APIs or SPIs defined in the terms of the business logic\. This way it will not depend on anything at all and any component will be replaceable with another implementation or a [stub/mock](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub)\.

<ins>Pros</ins>: 

- Vendor lock\-in is ruled out\.
- A component may be replaced through to the very end of the system’s life cycle\.
- Stubs and mocks are supported for testing and local or early development\.
- It is possible to provide multiple implementations of a component\.


<ins>Cons</ins>: 

- Some extra effort is required to define and use the interfaces\.
- There is performance degradation, mostly due to lost optimization opportunities\.


## Add an Interpreter \(support Scripts\)

<figure style="text-align:center">
<a href="/Evolutions/Monolith/Monolith%20to%20Interpreter.png" style="outline:none">
<img src="/Evolutions/Monolith/Monolith%20to%20Interpreter.png" alt="Monolith to Interpreter" style="width:100%"/>
</a>
</figure>

<ins>Patterns</ins>: [Scripts aka Interpreter]({{< relref "../../part-5--implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) \([Microkernel]({{< relref "../../part-5--implementation-metapatterns/microkernel.md" >}}) \([Plugins]({{< relref "../../part-5--implementation-metapatterns/plugins.md" >}})\)\)\.

<ins>Goal</ins>: allow the system’s users to implement their own business logic\.

<ins>Prerequisite</ins>: the domain is representable in high\-level terms\.

*Interpreter* lets the users develop high\-level business logic from scratch by programming interactions of pre\-defined building blocks which are implemented in the core of the system\. That provides unparalleled flexibility at the cost of performance and design complexity\.

<ins>Pros</ins>: 

- Perfect flexibility and customizability for every user\.
- The high\-level business logic is written in high\-level terms, making it fast to develop and easy to grasp\.


<ins>Cons</ins>: 

- Requires much effort to design correctly\.
- There may be a heavy performance penalty if the API is too fine\-grained\.
- Testability may be an issue\.


<nav>

| \<\< [Evolutions of a Monolith that make Services]({{< relref "../../part-7--appendices/appendix-e--evolutions/evolutions-of-a-monolith-that-make-services.md" >}}) | ^ [Appendix E\. Evolutions\.]({{< relref "../../part-7--appendices/appendix-e--evolutions/_index.md" >}}) ^ | [Evolutions of Shards that share data]({{< relref "../../part-7--appendices/appendix-e--evolutions/evolutions-of-shards-that-share-data.md" >}}) \>\> |
| --- | --- | --- |

</nav>



