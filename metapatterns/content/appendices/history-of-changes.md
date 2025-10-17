+++
weight = 8
title = "History of changes"
description = "The list of versions of Architectural Metapatterns and its predecessors."
[sitemap]
  disable = true
bookSearchExclude = true
+++

# History of changes {anchor=false}

0\.1 \(2020\) – Description of my semisynchronous *Proactor* architecture for a VoIP gateway, published by dou\.ua\. It received very positive feedback and lots of comments from the community\.

0\.2 \(2020\) – [The same in a more official style](http://www.hillside.net/plop/2020/papers/poltorak.pdf) for the \(Corona\-\)PLoP’20 conference\.

0\.3 \(2021\) – Comparison of choreography and orchestration for dou\.ua\. No impact\.

0\.4 \(2022\) – A series of 5 articles that looked into local and distributed architectures by applying the actor model\. Positive feedback from dou\.ua, but the series was interrupted by the war\.

0\.5 \(2023\) – [The same series in English](https://medium.com/itnext/introduction-to-software-architecture-with-actors-part-1-89de6000e0d3), published by ITNEXT and upvoted by r/softwarearchitecture\.

0\.6 \(2023\) – I attempted to rebuild the series for InfoQ but the first article was rejected as impractical \(technology\-agnostic\)\.

0\.7 \(09\-2024\) – [Chapters from this book](https://medium.com/itnext/the-list-of-architectural-metapatterns-ed64d8ba125d), published by ITNEXT\. Some of them were boosted by Medium\.

0\.8 \(11\-2024\) – The complete book as a pdf\. Clients were changed to mid\-brown\. Detailed evolutions were moved to the appendix\. Rejected by Manning \(the free license and color diagrams make the book unprofitable\) and O’Reilly \(it would get in the way of their bestsellers\)\. Ignored by Addisson\-Wesley\.

0\.9 \(12\-2024\) – Integrated patterns from \[[DDS]({{< relref "../appendices/books-referenced.md#dds" >}}), [LDDD]({{< relref "../appendices/books-referenced.md#lddd" >}}), [SAHP]({{< relref "../appendices/books-referenced.md#sahp" >}})\] and Internet sources, mostly affecting [*Shards*]({{< relref "../basic-metapatterns/shards.md" >}}), [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}), [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) and [*Hexagonal Architecture*]({{< relref "../implementation-metapatterns/hexagonal-architecture.md" >}})\. Added diagrams for [*Polyglot Persistence* with derived storage]({{< relref "../fragmented-metapatterns/polyglot-persistence.md#variants-with-derived-storage" >}}) and detailed evolutions for [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}})\. Downgraded [analytical chapters]({{< relref "../analytics/comparison-of-architectural-patterns/_index.md" >}}) to sections and added a couple of new ones\. Extended the [ambiguous patterns chapter]({{< relref "../analytics/ambiguous-patterns.md" >}})\. Improved the structure of the variants sections of metapatterns: now each synonym has a short description\. Fixed alignment of text and figures\. Liked by [r/softwarearchitecture](https://www.reddit.com/r/softwarearchitecture/comments/1hi377v/free_book_architectural_metapatterns_the_pattern/)\. Rejected by The Pragmatic Programmer \(they want “hands\-on, actionable content”\)\. Ignored by No Starch Press and Packt\.

1\.0 \(04\-2025\) – Integrated \[[DEDS]({{< relref "../appendices/books-referenced.md#deds" >}})\]\. Integration logic \(use cases\) is now in green\. Added [*MVC*\-related patterns]({{< relref "../implementation-metapatterns/hexagonal-architecture.md#examples--separated-presentation" >}}) and a section on [Programming and architectural paradigms]({{< relref "../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md" >}})\. Replaced the chapter on [control and processing](https://medium.com/itnext/control-and-processing-software-9011fee8bc66) with a new one about [Four kinds of software]({{< relref "../foundations-of-software-architecture/four-kinds-of-software.md" >}}) and added another one called [The heart of software architecture]({{< relref "../analytics/the-heart-of-software-architecture/_index.md" >}})\. Made minor changes all over the book\. Now I know how to generate a table of contents for both EPUB and PDF versions\. Ignored by Wikibooks\.

1\.1 \(07\-2025\) – Lars Noodén edited the book, fixing my poor English\. Patterns are now in *Title Case Italics*\. [*Domain\-Oriented Microservice Architecture*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md#domain-oriented-microservice-architecture-doma" >}}) was added\. There are now short explanation sections \(in gray\) throughout the book\.

1\.1\.1 \(10\-2025\) – Renamed [Appendix E]({{< relref "../appendices/evolutions-of-architectures/_index.md" >}}) and its subchapters\. Made other minor SEO improvements and bugfixes\.