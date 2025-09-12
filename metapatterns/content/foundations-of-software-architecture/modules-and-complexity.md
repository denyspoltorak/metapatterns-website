+++
weight = 1
title = "Modules and complexity"
description = "Complexity is the number of concepts which one must keep in mind to understand a system. Breaking a system into components may lower its complexity."
+++

# Modules and complexity

<aside>

> This chapter is loosely based on [A Philosophy of Software Design](https://blog.pragmaticengineer.com/a-philosophy-of-software-design-review/) by John Ousterhout and [my article](https://medium.com/itnext/introduction-to-software-architecture-with-actors-part-1-89de6000e0d3)\.

</aside>

Any software system which we encounter is very likely to be too complex to comprehend all at once – the human mind is incapable of discerning a large number of entities and their relations\. It tends to simplify reality by building abstractions: as soon as we define the many shiny pieces of metal, glass and rubber as a ‘car’ we can identify ‘highways’, ‘parking spaces’ and ‘passengers’ – we live in a world of the abstractions which we create\. In the same way the software we write is built of services, processes, files, classes, procedures – modules that conceal the swarm of bits and pieces which we are powerless against\. Let’s reflect on that\.

## Concepts and complexity

Any system is comprised of *concepts* – notions defined in terms of other concepts\. For example, if you are implementing a phonebook, you deal with *first* and *last names*, *numbers*, *sorting*, and *search*, which one must always keep in mind for any phonebook\-related development task – just because requirements for the phonebook are described in terms of those concepts and their relations\.

In the code high\-level concepts are embodied as services, modules or directories while lower\-level concepts match to classes, API methods or source files\.

Concepts are important because it is their number \(or the number of the corresponding classes and methods\) that defines the *complexity* of a system – the cognitive load which developers of the system face\. If the programmers grasp the behavior of a component they work on in detail they tend to [become extremely productive](https://www.quora.com/What-are-some-habits-of-10x-programmers) and are often able to find [simple solutions for seemingly complex tasks](https://realmensch.org/2017/08/25/the-parable-of-the-two-programmers/)\. Otherwise the development is slow and requires extensive testing because the programmers are [unsure of how their changes affect the system’s behavior](https://news.ycombinator.com/item?id=18442941)\.

<figure style="text-align:center">
<a href="/Intro/Modules-1.png" style="outline:none">
<img src="/Intro/Modules-1.png" alt="Modules-1" style="width:52%"/>
</a>
</figure>

Figure 1: Complexity correlates with the number of entities\.

## Modules, encapsulation and bounded context

Let’s return to our example\. As you implement the phonebook you find out that sorting and search are way more complex than you originally thought\. Once you prepare to enter the international market you are in [deep trouble](https://en.wikipedia.org/wiki/Alphabetical_order#Language-specific_conventions)\. Some telephony providers send 7\-digit numbers, others use 10 digits, still others – 13 digits \(with either “\+” or “0” for the first character\)\. German has “ß” which is identical to “ss” while Japanese uses two alphabets simultaneously\. Once you start reading standards, implementing all the weird behavior and responding to user complaints you feel that your phonebook implementation is drowning in the unrelated logic of foreign alphabets’ special cases\. You need *encapsulation*\.

Enter *modules*\. A module wraps several concepts, effectively hiding them from external users, and exposes a simplified view of its contents\. Introducing modules splits a complex system into several, usually less complex, parts\.

<figure style="text-align:center">
<a href="/Intro/Modules-2.png" style="outline:none">
<img src="/Intro/Modules-2.png" alt="Modules-2" style="width:66%"/>
</a>
</figure>

Figure 2: Dividing a system into modules, bounded contexts highlighted\.

This diagram has several important points to note:

- Modules create new concepts for their *public APIs*\.
- The API entry points add to the complexity of *both* the owner module and its clients\.
- The total number of concepts in the system has increased \(from 18 to 22\) but the highest complexity in the system has dropped \(from 18 to 15\)\.


Here we see how introducing modularity applies the [divide and conquer](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) approach to lessen the cognitive load of working on any part of a system at the cost of a small increase in the total amount of work to be done\.

In our phonebook example the peculiarities \(including case sensitivity\) of the locale\-aware string comparison and alphabetical sorting of contact names should better be kept behind a simple string comparison interface to relieve the programmer of the phonebook engine of the complexity of supporting foreign languages\.

Modules represent *bounded contexts* \[[DDD]({{< relref "../appendices/books-referenced.md#ddd" >}})\] – areas of the knowledge about a system that operate distinct sets of terms\. In the case of phonebook the *collation* and *case sensitivity* do not matter for the phonebook engine – they are defined only in the context of language support\. On the other hand, *matching a contact by number* is not defined in the language support module – that term exists only in the phonebook engine\. It is the complexity of the current bounded context that a programmer struggles with\.

Apart from dividing the problem into simpler subproblems, modules open the path to a few extra benefits:

- *Code reuse*\. A well\-written module that implements something generic may be used in multiple projects\.
- *Division of labor*\. Once a system is split into modules and each module is assigned one or more programmers, development is efficiently parallelized\.
- *High\-level concepts*\. Some cases allow for merging several concepts of the original problem into higher\-level aggregates, further reducing the complexity:


<figure style="text-align:center">
<a href="/Intro/Modules-3.png" style="outline:none">
<img src="/Intro/Modules-3.png" alt="Modules-3" style="width:64%"/>
</a>
</figure>

Figure 3: Merged two API concepts in the green module\.

For example, the original definition of a phonebook contained *first name* and *last name*\. Once we separate the language support into a dedicated module, we may find out that various locales differ in the way they represent contacts: some \(USA\) use ‘first name \+ last name’ while others \(Japan\) need ‘last name \+ first name’\. If we want to abstract ourselves from that detail, we should use a new concept of *full name* which conjoins first and last names in a locale\-specific way\. Such a change actually simplifies some of the phonebook’s representation logic and code as it replaces two concepts with one\.

## Coupling and cohesion

We need to learn a couple of new concepts in order to use modules efficiently:

*Coupling* is a measure of the number \(density\) of connections between modules relative to the modules’ sizes\.

*Cohesion* is a measure of the number \(density\) of connections inside a module relative to the module’s size\.

The rule of thumb is to aim for *low coupling and high cohesion*, meaning that each module should encapsulate a cluster of related \(intensely interacting\) concepts\. This is how we have split the system in figures 2 and 3\. Now let’s see what happens if we violate the rules:

<figure style="text-align:center">
<a href="/Intro/Modules-4.png" style="outline:none">
<img src="/Intro/Modules-4.png" alt="Modules-4" style="width:75%"/>
</a>
</figure>

Figure 4: The upper modules are tightly coupled\.

Splitting a cohesive module \(a cluster of concepts that interact with each other\) yields two strongly coupled modules\. That’s what we wanted, except that each of the new modules is nearly as complex as the original one\. Meaning, that we now face two hard tasks instead of one\. Also, the system’s performance may be poor because communication between modules is rarely optimal, and we’ve got too much of that\.

<figure style="text-align:center">
<a href="/Intro/Modules-5.png" style="outline:none">
<img src="/Intro/Modules-5.png" alt="Modules-5" style="width:64%"/>
</a>
</figure>

Figure 5: The lower module has low cohesion\.

What happens if we put several clusters of concepts in the same module? Nothing too evil happens with small modules – the module gets a higher complexity than each of its constituents, but lower than their sum\. In practice, multiple unrelated functions are often gathered in a ‘utils’ or ‘tools’ file or directory to alleviate *operational complexity*\.

## Development and operational complexity

What we discussed above is *structural* or *development complexity* – the number of concepts and rules inside a bounded context\. However, we also need to understand operations and components of the system as a whole, leading to *operational* or *integration complexity*:

- Does this new requirement fit into an existing module or does it call for writing a dedicated one?
- Which libraries with known security vulnerabilities do we use?
- Is there any way to cut our cloud services cost?
- 1% of requests time out\. Would you please investigate that?
- My team needs to implement this and that\. Do we have something fit for reuse?
- What the \*\*\*\* is [that global variable](https://news.ycombinator.com/item?id=18442941) about?
- Do we really need this code in production?
- I need to change the behavior of that shared component a little bit\. Any objections?


When there are hundreds or thousands of modules deployed nobody knows the answers\. That’s similar to the case of one needing to do something in Linux: hundreds of tools are pre\-installed and thousands more are available as packages, but the only real way forward is first searching the web for your needs, then trying two or three recipes from the results to see which one fits your setup\. Unfortunately, Google does not index your company’s code\.

## Composition of modules

A module may encapsulate not only individual concepts, but even other modules\. That is not surprising as an OOP class is a kind of module – it also has public methods and private members\. Hiding a module inside another one removes it from the global scope, decreasing the operational complexity of the system – now it is not the system’s architect’s responsibility but the responsibility of the maintainer of the outer module who cares about the inner module\. On one hand, that builds a manageable hierarchy in both the organization and the code\. On the other hand, code reuse and many optimizations become nearly impossible as internal modules are hardly known organization\-wide:

<figure style="text-align:center">
<a href="/Intro/Modules-6.png" style="outline:none">
<img src="/Intro/Modules-6.png" alt="Modules-6" style="width:58%"/>
</a>
</figure>

Figure 6: Composition of modules prevents reuse\.

If the functionality of our internal module is needed by our clients, we have two bad options to choose from:

## Forwarding and duplication

<figure style="text-align:center">
<a href="/Intro/Modules-7.png" style="outline:none">
<img src="/Intro/Modules-7.png" alt="Modules-7" style="width:59%"/>
</a>
</figure>

Figure 7: Forwarding the API of an internal module\.

We can add the API of a module which we encapsulate to our public API and forward its calls to the internal module\. However, that increases the complexity and lowers the cohesion of our module – now each client of our module is also exposed to the details of the methods of the module we have encapsulated even if they are not interested in using it\.

<figure style="text-align:center">
<a href="/Intro/Modules-8.png" style="outline:none">
<img src="/Intro/Modules-8.png" alt="Modules-8" style="width:78%"/>
</a>
</figure>

Figure 8: Duplicating an internal module\.

Another bad option is to let the clients that need a module which we encapsulate duplicate it and own the copies as their own submodules\. This relieves us of any shared responsibility, lets us modify and misuse our internals in any way we like, but violates [a couple](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)) [of rules](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) of common sense\.

Both approaches, namely keeping all the modules in the global scope and encapsulating utility modules through composition, found their place in history \[[FSA]({{< relref "../appendices/books-referenced.md#fsa" >}})\]\. [*Service\-Oriented Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) was based on the idea of reuse but fell prey to the complexity of its [*Enterprise Service Bus*]({{< relref "../extension-metapatterns/combined-component.md#enterprise-service-bus-esb" >}}) which had to account for all the interactions \(API methods\) in the system\. In response, the [*Microservices*]({{< relref "../basic-metapatterns/services.md#microservices" >}}) approach turned the tide in the opposite direction: its proponents disallowed sharing any resources or code between services to enforce their decoupling\.

## Summary

*Complexity* is the number of *concepts* and their relations which one must remember to work efficiently\. A *module* hides some of the concepts from its users but creates new concepts \(its *interface*\)\. *Coupling* is the measure of dependencies between the modules, while *cohesion* is the same for the concepts inside a module\. We prefer *low coupling and high cohesion* to group related things together\.

Having too many modules causes trouble for the system’s maintainers\. A module may contain other modules\. When a client wants to use a submodule, the wrapping module may extend its interface to forward client’s requests to the submodule or the client may deploy a copy of the submodule for its own use\. Both approaches gave rise to prominent architectures\.

<nav>

| \<\< [Foundations of software architecture]({{< relref "../foundations-of-software-architecture/_index.md" >}}) | ^ [Foundations of software architecture]({{< relref "../foundations-of-software-architecture/_index.md" >}}) ^ | [Forces, asynchronicity, and distribution]({{< relref "../foundations-of-software-architecture/forces--asynchronicity--and-distribution.md" >}}) \>\> |
| --- | --- | --- |

</nav>



