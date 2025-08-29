+++
weight = 5
title = "About this book"
+++

# About this book

When I was learning programming, there was [*Gang of Four*](https://en.wikipedia.org/wiki/Design_Patterns)\. The book promised to teach software design, and it did to an extent with the case study provided\. However, the patterns it described were merely random tools which had little in common\. After several years, having reinvented [*Hexagonal Architecture*](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)) along the way, I learned about [*Pattern\-Oriented Software Architecture*](https://en.wikipedia.org/wiki/Pattern-Oriented_Software_Architecture)\. The series had many more intriguing patterns, and promised to provide a *system of patterns* or a *pattern language*, but failed to build an intuitive whole\. Then there were specialized books with [*Domain\-Driven Design*](https://en.wikipedia.org/wiki/Domain-driven_design) and *Microservices* patterns\. There was the [*Software Architecture Patterns*](https://www.oreilly.com/content/software-architecture-patterns/) primer by Mark Richards\. Its simplicity felt great, but it had only 5 architectural styles, while his next book, *Fundamentals of Software Architecture*, dived too deeply into practical details and examples to be easily grasped\.

Now, having leisure thanks to the war, burnout, unemployment and depression I have had a chance to collect architectural patterns from multiple sources and build a taxonomy of architectures\. My goal was to write the very book I lacked in those early years: a shallow but intuitive overview of all the software and system architectures as used in practice, their properties and relations\. I hope that it will be of some help both to novice programmers as a kind of a primer on the principles of high\-level software design and to adept architects by reminding them of the big picture outside of their areas of expertise\.

The book is mostly technology\-agnostic\. It does not answer practical questions like “Which database should I use?” Instead it inclines towards the understanding of “When should I use a shared database?” Any specific technologies ~are easy to google~ can be found ~over the Internet~ somewhere in the Noosphere\.

This book started as a rather small project to prove that patterns can be intuitively classified \(*These nightmarish creatures* can *be felled\! They* can *be beaten\!*\) but grew into a multifaceted compendium of a hundred or so architectures and architectural patterns\. It is grounded in the idea that software and system architecture evolves naturally, as opposed to being scientifically planned\. Thus, the architectures may exhibit [fractal features]({{< relref "../part-1--foundations/arranging-communication.md#programming-and-architectural-paradigms" >}}), just like those in biology – merely because the [set of guidelines and forces]({{< relref "../part-6--analytics/the-heart-of-software-architecture.md#cohesers-and-decouplers" >}}) remains the same for most systems that range from low\-end embedded devices to world\-wide financial networks\. Moreover, in some cases we can see the same patterns applied to hardware design\.

The idea of unifying software and system architecture is heretical\. I am well aware of that\. Still, the industry is in the early stage of alchemy these days: the same things are sold under multitudes of names, being remarketed or reinvented every decade\. If this book manages to provide rules of thumb, similar to those of biology \(a bat is a mammal, thus it should run on all four, while ostriches, as birds, must fly to Europe each spring\), I will be happy with that\. *Science makes progress funeral by funeral*\.

The latest version of the book is available for free on [GitHub](https://github.com/denyspoltorak/publications/blob/main/ArchitecturalMetapatterns) and [LeanPub](https://leanpub.com/metapatterns)\. As there is no one who has practiced all the known architectures, it will be full of mistakes\. I rely on your goodwill to correct them and improve the text\. Critical reviews are warmly welcome: please write an [email](mailto:descri@gmail.com) or contact me [on LinkedIn](https://www.linkedin.com/in/denyspoltorak/)\.

## Structure of the book

The [first chapter]({{< relref "../introduction/metapatterns.md" >}}) explains the main idea which makes this book different from others\. The following chapters in the [first part]({{< relref "../part-1--foundations/_index.md" >}}) touch on several general topics that are referenced throughout the book\.

The next [four parts]({{< relref "../part-2--basic-metapatterns/_index.md" >}}) iterate over *metapatterns* \(clusters of closely related architectural patterns\), starting with the simplest one, namely [*Monolith*]({{< relref "../part-2--basic-metapatterns/monolith.md" >}}), then heading towards more complex systems that may be derived from *Monolith* by repeatedly dissecting it with interfaces\. Each chapter describes a group of related patterns that share benefits and drawbacks, adds in a few references to books and websites, and summarizes the ways the patterns can be transformed into other architectures\. The format of these chapters is described in [Appendix F]({{< relref "../part-7--appendices/appendix-f--format-of-a-metapattern.md" >}})\.

The [sixth part]({{< relref "../part-6--analytics/_index.md" >}}) of the book is analytics – the fruits of the pattern classification from the earlier parts\.

Finally, there are appendices\. [Appendix B]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}}) is the list of the books referenced, [Appendix E]({{< relref "../part-7--appendices/appendix-e--evolutions.md" >}}) contains detailed evolutions of patterns and [Appendix I]({{< relref "../part-7--appendices/appendix-i--index-of-patterns.md" >}}) is the index of the patterns found in the book\.

## Diagrams

This book makes heavy use of diagrams – to the extent that it can be treated as a kind of visual novel\. As it is mostly made of patterns, and *each pattern is an island*, it must not be read sequentially – instead, the reader is advised to use the plentiful cross\-links to open whatever \(if any\) content found to be intriguing and check the corresponding diagram\. If it gets your attention, you may read the text below it\. If you like the text, you may scroll up or down to see if there are more funny diagrams nearby\.

The diagrams are *NoUML* and most of them belong to one of the following kinds:

<p align="center">
<img src="/Misc/Diagrams.png" alt="Diagrams" width=100%/>
</p>

Please refer to the [following chapter]({{< relref "../introduction/metapatterns.md#the-system-of-coordinates" >}}) for the legend and the system of coordinates\.

## Notation

- Pattern names are given in [*Title Case Italics*]({{< relref "../part-7--appendices/appendix-i--index-of-patterns.md" >}}) and usually link to the pattern’s definition\.
- The first mention of a term or a name of a pattern component is *italicized*\.
- *Quotes and puns are in full italics*\.
- Book references are \[[BRACKETED]({{< relref "../part-7--appendices/appendix-b--books-referenced.md" >}})\] and link to the list of the books in Appendix B\.
- > Supplementary explanations are grayed\-out\.


Many patterns match terms of the common language – indeed, as a pattern is a generalization of human experience, the more widespread a notion, the faster it is turned into a pattern\. Such general\-use terms, e\.g\. layers, services or pipeline, are usually not indicated in any way to preserve the overall readability\.

## The architectural religions

There are several schools of software architecture:

1.  The believers in [SOLID](https://en.wikipedia.org/wiki/SOLID)\.
2.  The followers of [eight qualities](https://iso25000.com/index.php/en/iso-25000-standards/iso-25010), [five views](https://en.wikipedia.org/wiki/4%2B1_architectural_view_model) and [as\-many\-as\-one\-gets certifications](https://en.wikipedia.org/wiki/Enterprise_architecture_framework#Types_of_enterprise_architecture_framework)\.
3.  The aspirants to the nameless way of [patterns](https://en.wikipedia.org/wiki/Software_design_pattern)\.


In my opinion:

1.  SOLID is a silver bullet that tends to produce a [DDD\-layered kind of Hexagonal Architecture](https://herbertograca.com/2017/09/28/clean-architecture-standing-on-the-shoulders-of-giants/)\. It lacks the agility of pluralism found with evolutionary ecosystems\.
2.  Architectural frameworks are overcomplicated thus hard to understand and inflexible\.
3.  Patterns are like a kind of toolbox, the one which a mechanic is often seen carrying around\. A skilled craftsman knows best uses of his tools, and can invent new instruments if something is missing in the standard toolset\. However, the toolset’s size should be limited for the tools to be familiar to the practitioner and easily carried around\.


It is likely that those approaches are best used with systems of different sizes: SOLID is aimed at stand\-alone application design while the heavy frameworks and certifications suit distributed enterprise architectures\. In such a worldview patterns span everything in between the two extremes\.

> Patterns of software architecture are abstract just like [Plato’s Ideas or Forms](https://en.wikipedia.org/wiki/Theory_of_forms) in philosophy or classes in object\-oriented programming\. There is only one instance of each given pattern, which is a general idea or a very high\-level blueprint for every implementation of the pattern ever seen in the code\. 

## What’s wrong with patterns

*Too much information is no information* or, as they say, *what is not remembered never existed*\. There are literally thousands of patterns described for software and system architectures\. Nobody knows them all and nobody cares to know them all \(if you say you do, you must have already read [the Pattern Languages of Programs archives](https://hillside.net/index.php/past-plop-conferences)\. Have you? Neither have I\)\. Hundreds of patterns are generated yearly in just the conferences alone, not to mention the books and software engineering websites\. Old patterns get [rebranded or forgotten and reinvented](https://datatracker.ietf.org/doc/html/rfc1925)\. This is especially true for the discrepancy between the pattern names in software architecture and system architecture\. The new *N\-tier* is just good old *Layers* under the hood, isn’t it?

This undermines the original ideas which brought in the patterns hype:

1.  *Patterns as a ubiquitous language*\. Nowadays similar, if not identical, patterns bear different names, and some of them are too obscure to be ever heard of \(see [the PLoP archives](https://hillside.net/index.php/past-plop-conferences)\)\.
2.  *Patterns as a vessel for knowledge transfer*\. If an old pattern is reinvented or plagiarized, most of the old knowledge is lost\. There is no continuity of experience\.
3.  *Pattern language as the ultimate architect’s tool*\. As patterns are re\-invented, so are pattern languages\. At best, we have domain\-specific or architecture\-limited \([*DDD*](https://en.wikipedia.org/wiki/Domain-driven_design), [*Microservices*](https://microservices.io/patterns/index.html)\) systems of patterns\. There is no *single unified vision* which pattern enthusiasts of old promised to provide\.


Have we been fooled?

## TLDR

Compare [*Firewall*]({{< relref "../part-3--extension-metapatterns/proxy.md#firewall-api-rate-limiter-api-throttling" >}}) and [*Response Cache*]({{< relref "../part-3--extension-metapatterns/proxy.md#response-cache-read-through-cache-write-through-cache-write-behind-cache-cache-caching-layer-distributed-cache-replicated-cache" >}})\. Both represent a system to its users and implement generic aspects of the system’s behavior\. Both are [*Proxies*]({{< relref "../part-3--extension-metapatterns/proxy.md" >}})\.

Take [*Saga Execution Component*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}) and [*API Composer*]({{< relref "../part-3--extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}})\. Both are high\-level services that make a series of calls into an underlying system – they *orchestrate* it\. Both are [*Orchestrators*]({{< relref "../part-3--extension-metapatterns/orchestrator.md" >}})\.

It’s that simple and stupid\. We can classify architectural patterns\.

| \<\< [Introduction]({{< relref "../introduction/_index.md" >}}) | ^ [Introduction]({{< relref "../introduction/_index.md" >}}) ^ | [Metapatterns]({{< relref "../introduction/metapatterns.md" >}}) \>\> |
| --- | --- | --- |


