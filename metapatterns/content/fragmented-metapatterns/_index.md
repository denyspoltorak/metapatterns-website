+++
weight = 5
title = "Fragmented metapatterns"
description = "Fragmented architectures make use of small specialized components. Examples include Layered Services, BFF, SOA, Polyglot Persistence, and Hierarchy."
bookCollapseSection = true
[sitemap]
  priority = 0.2
+++

# Fragmented metapatterns

There are several patterns with no system\-wide layers\. Some of them incorporate two or three orthogonal domains which vary in abstractness to the extent that a service \(limited to a subdomain\) of one domain acts as a layer for another domain\.

### [Layered Services]({{< relref "../fragmented-metapatterns/layered-services.md" >}})

<figure>
<a href="/diagrams/Contents/Layered%20Services.png">
<img src="/diagrams/Contents/Layered%20Services.png" alt="Layered Services" loading="lazy" width="2109" height="634" style="width:100%"/>
</a>
</figure>

*Layered Services* is an umbrella metapattern that highlights implementation details of *Services*, *Pipeline*, or *Monolith*\.

*<ins>Includes</ins>*: Orchestrated Three\-Layered Services, Choreographed Two\-Layered Services, Command Query Responsibility Segregation \(CQRS\)\.

### [Polyglot Persistence]({{< relref "../fragmented-metapatterns/polyglot-persistence.md" >}})

<figure>
<a href="/diagrams/Contents/Polyglot%20Persistence.png">
<img src="/diagrams/Contents/Polyglot%20Persistence.png" alt="Polyglot Persistence" loading="lazy" width="1903" height="604" style="width:100%"/>
</a>
</figure>

*Polyglot Persistence* is about using multiple data stores which differ in roles or technologies\. Each of the upper\-level components may have access to any data store\. Each data store is a *Shared Repository*\.

*<ins>Includes</ins>*: specialized databases, private and shared databases, data file, Content Delivery Network \(CDN\); read\-only replica, Reporting Database, CQRS View Database, Memory Image, Query Service, search index, historical data, Cache\-Aside\.

### [Backends for Frontends]({{< relref "../fragmented-metapatterns/backends-for-frontends--bff-.md" >}})

<figure>
<a href="/diagrams/Contents/Backends%20for%20Frontends.png">
<img src="/diagrams/Contents/Backends%20for%20Frontends.png" alt="Backends for Frontends" loading="lazy" width="1877" height="735" style="width:100%"/>
</a>
</figure>

*Backends for Frontends* feature a service \(*BFF*\) for each kind of the system’s client\. A *BFF* may be a *Proxy*, *Orchestrator* or both\. Each *BFF* communicates with all the components below it\. The pattern looks like multiple *Proxies* or *Orchestrators* deployed in parallel\.

*<ins>Includes</ins>*: Layered Microservice Architecture\.

### [Service\-Oriented Architecture]({{< relref "../fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})

<figure>
<a href="/diagrams/Contents/Service-Oriented%20Architecture.png">
<img src="/diagrams/Contents/Service-Oriented%20Architecture.png" alt="Service-Oriented Architecture" loading="lazy" width="1847" height="784" style="width:100%"/>
</a>
</figure>

*SOA* comprises three or four layers of services, with each layer making a domain\. The upper layer contains *Orchestrators* which are often client\-specific, just like *BFFs*\. The second layer incorporates business rules and is divided into business subdomains\. The lower layer\(s\) are libraries and utilities, grouped by functionality and technologies\. Any component may use \(orchestrate\) anything below it\.

*<ins>Includes</ins>*: Segmented Architecture; distributed monolith, enterprise SOA\.

### [Hierarchy]({{< relref "../fragmented-metapatterns/hierarchy.md" >}})

<figure>
<a href="/diagrams/Contents/Hierarchy.png">
<img src="/diagrams/Contents/Hierarchy.png" alt="Hierarchy" loading="lazy" width="1714" height="594" style="width:100%"/>
</a>
</figure>

Some domains allow for hierarchical composition where the functionality is spread throughout a tree of components\.

*<ins>Includes</ins>*: Orchestrator of Orchestrators, Presentation\-Abstraction\-Control \(PAC\) and Hierarchical Model\-View\-Controller \(HMVC\), Bus of Buses, and Cell\-Based \(Microservice\) Architecture \(WSO2 version\) \(Services of Services\)\.

<nav>

| \<\< [Combined Component]({{< relref "../extension-metapatterns/combined-component.md" >}}) | ^ [The pattern language of software architecture]({{< relref "../_index.md" >}}) ^ | [Layered Services]({{< relref "../fragmented-metapatterns/layered-services.md" >}}) \>\> |
| --- | --- | --- |

</nav>