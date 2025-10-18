+++
weight = 4
title = "Evolutions of a Monolith that rely on Plugins"
description = "Plugins or Interpreter make a monolithic component customizable. Hexagonal Architecture protects its business logic from unstable external dependencies. "
images = ["/diagrams/Web/Favicon-plain.png"]
[sitemap]
  priority = 0.3
+++

# Evolutions of a Monolith that rely on Plugins {anchor=false}

The last group of evolutions which we review does not really change the monolithic nature of the application\. Instead, its goal is to improve the *customizability* of the [*Monolith*]({{< relref "../../basic-metapatterns/monolith.md" >}}):

- Vanilla [*Plugins*]({{< relref "../../implementation-metapatterns/plugins.md" >}}) is the most direct approach which relies on replaceable bits of logic\.
- [*Hexagonal Architecture*]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) is a subtype of *Plugins* which is all about isolating the main code from any third\-party components it uses\.
- [*Scripts*]({{< relref "../../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) is a kind of [*Microkernel*]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) – yet another subtype of *Plugins* – which gives users of the system full control over its behavior\.


## Support plugins

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Plugins.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Plugins.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Plugins.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Plugins.png" alt="Monolith to Plugins" loading="lazy" width="1003" height="363" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Plugins]({{< relref "../../implementation-metapatterns/plugins.md" >}})\.

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

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Hexagonal.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Hexagonal.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Hexagonal.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Hexagonal.png" alt="Monolith to Hexagonal" loading="lazy" width="1007" height="323" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Hexagonal Architecture]({{< relref "../../implementation-metapatterns/hexagonal-architecture.md" >}}) \([Plugins]({{< relref "../../implementation-metapatterns/plugins.md" >}})\)\.

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

<figure>
<a href="/diagrams/Evolutions/Monolith/Monolith%20to%20Interpreter.png">
<picture>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Interpreter.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/Monolith/Monolith%20to%20Interpreter.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/Monolith/Monolith%20to%20Interpreter.png" alt="Monolith to Interpreter" loading="lazy" width="1087" height="323" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Scripts aka Interpreter]({{< relref "../../implementation-metapatterns/microkernel.md#interpreter-script-domain-specific-language-dsl" >}}) \([Microkernel]({{< relref "../../implementation-metapatterns/microkernel.md" >}}) \([Plugins]({{< relref "../../implementation-metapatterns/plugins.md" >}})\)\)\.

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
