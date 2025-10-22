+++
weight = 5
title = "Fragmented metapatterns"
description = "Fragmented architectures make use of small specialized components. Examples include Layered Services, BFF, SOA, Polyglot Persistence, and Hierarchy."
images = ["/diagrams/Web/og/Favicon-plain.png"]
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Fragmented metapatterns {anchor=false}

There are several patterns with no system\-wide layers\. Some of them incorporate two or three orthogonal domains which vary in abstractness to the extent that a service \(limited to a subdomain\) of one domain acts as a layer for another domain\.

### [Layered Services]({{< relref "../fragmented-metapatterns/layered-services.md" >}})

<figure>
<a href="/diagrams/Contents/Layered%20Services.png">
<picture>
<source srcset="/diagrams/Contents/Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Layered%20Services.png" alt="Layered Services" loading="lazy" width="1133" height="343" style="width:100%"/>
</picture>
</a>
</figure>

[*Layered Services*]({{< relref "../fragmented-metapatterns/layered-services.md" >}}) is an umbrella metapattern that highlights implementation details of [*Services*]({{< relref "../basic-metapatterns/services.md" >}}), [*Pipeline*]({{< relref "../basic-metapatterns/pipeline.md" >}}), or [*Monolith*]({{< relref "../basic-metapatterns/monolith.md" >}})\.

*<ins>Includes</ins>*: Orchestrated Three\-Layered Services, Choreographed Two\-Layered Services, Command Query Responsibility Segregation \(CQRS\)\.

### [Polyglot Persistence]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})

<figure>
<a href="/diagrams/Contents/Polyglot%20Persistence.png">
<picture>
<source srcset="/diagrams/Contents/Polyglot%20Persistence.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Polyglot%20Persistence.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Polyglot%20Persistence.png" alt="Polyglot Persistence" loading="lazy" width="1023" height="303" style="width:100%"/>
</picture>
</a>
</figure>

[*Polyglot Persistence*]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}}) is about using multiple data stores which differ in roles or technologies\. Each of the upper\-level components may have access to any data store\. Each data store is a [*Shared Repository*]({{< relref "../extension-metapatterns/shared-repository.md" >}})\.

*<ins>Includes</ins>*: specialized databases, private and shared databases, data file, Content Delivery Network \(CDN\); read\-only replica, Reporting Database, CQRS View Database, Memory Image, Query Service, search index, historical data, Cache\-Aside\.

### [Backends for Frontends]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})

<figure>
<a href="/diagrams/Contents/Backends%20for%20Frontends.png">
<picture>
<source srcset="/diagrams/Contents/Backends%20for%20Frontends.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Backends%20for%20Frontends.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Backends%20for%20Frontends.png" alt="Backends for Frontends" loading="lazy" width="1003" height="383" style="width:100%"/>
</picture>
</a>
</figure>

[*Backends for Frontends*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}}) feature a service \(*BFF*\) for each kind of the system’s client\. A *BFF* may be a [*Proxy*]({{< relref "../extension-metapatterns/proxy.md" >}}), [*Orchestrator*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) or both\. Each *BFF* communicates with all the components below it\. The pattern looks like multiple *Proxies* or *Orchestrators* deployed in parallel\.

*<ins>Includes</ins>*: Layered Microservice Architecture\.

### [Service\-Oriented Architecture]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})

<figure>
<a href="/diagrams/Contents/Service-Oriented%20Architecture.png">
<picture>
<source srcset="/diagrams/Contents/Service-Oriented%20Architecture.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Service-Oriented%20Architecture.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Service-Oriented%20Architecture.png" alt="Service-Oriented Architecture" loading="lazy" width="983" height="401" style="width:100%"/>
</picture>
</a>
</figure>

[*SOA*]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}}) comprises three or four layers of services, with each layer making a domain\. The upper layer contains [*Orchestrators*]({{< relref "../extension-metapatterns/orchestrator.md" >}}) which are often client\-specific, just like [*BFF*]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})*s*\. The second layer incorporates business rules and is divided into business subdomains\. The lower layer\(s\) are libraries and utilities, grouped by functionality and technologies\. Any component may use \(orchestrate\) anything below it\.

*<ins>Includes</ins>*: Segmented Architecture; distributed monolith, enterprise SOA\.

### [Hierarchy]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})

<figure>
<a href="/diagrams/Contents/Hierarchy.png">
<picture>
<source srcset="/diagrams/Contents/Hierarchy.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Contents/Hierarchy.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Contents/Hierarchy.png" alt="Hierarchy" loading="lazy" width="907" height="303" style="width:100%"/>
</picture>
</a>
</figure>

Some domains allow for [hierarchical composition]({{< relref "../fragmented-metapatterns/hierarchy.md" >}}) where the functionality is spread throughout a tree of components\.

*<ins>Includes</ins>*: Orchestrator of Orchestrators, Presentation\-Abstraction\-Control \(PAC\) and Hierarchical Model\-View\-Controller \(HMVC\), Bus of Buses, and Cell\-Based \(Microservice\) Architecture \(WSO2 version\) \(Services of Services\)\.