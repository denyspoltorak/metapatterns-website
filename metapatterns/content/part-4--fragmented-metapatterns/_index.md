+++
weight = 5
title = "Part 4. Fragmented Metapatterns"
bookCollapseSection = true
+++

# Part 4\. Fragmented Metapatterns

There are several patterns with no system\-wide layers\. Some of them incorporate two or three orthogonal domains which vary in abstractness to the extent that a service \(limited to a subdomain\) of one domain acts as a layer for another domain\.

### [Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}})

<figure style="text-align:center">
<a href="/Contents/Layered%20Services.png" style="outline:none">
<img src="/Contents/Layered%20Services.png" alt="Layered Services" style="width:100%"/>
</a>
</figure>

*Layered Services* is an umbrella metapattern that highlights implementation details of *Services*, *Pipeline*, or *Monolith*\.

*<ins>Includes</ins>*: Orchestrated Three\-Layered Services, Choreographed Two\-Layered Services, Command Query Responsibility Segregation \(CQRS\)\.

### [Polyglot Persistence]({{< relref "../part-4--fragmented-metapatterns/polyglot-persistence.md" >}})

<figure style="text-align:center">
<a href="/Contents/Polyglot%20Persistence.png" style="outline:none">
<img src="/Contents/Polyglot%20Persistence.png" alt="Polyglot Persistence" style="width:100%"/>
</a>
</figure>

*Polyglot Persistence* is about using multiple data stores which differ in roles or technologies\. Each of the upper\-level components may have access to any data store\. Each data store is a *Shared Repository*\.

*<ins>Includes</ins>*: specialized databases, private and shared databases, data file, Content Delivery Network \(CDN\); read\-only replica, Reporting Database, CQRS View Database, Memory Image, Query Service, search index, historical data, Cache\-Aside\.

### [Backends for Frontends]({{< relref "../part-4--fragmented-metapatterns/backends-for-frontends--bff-.md" >}})

<figure style="text-align:center">
<a href="/Contents/Backends%20for%20Frontends.png" style="outline:none">
<img src="/Contents/Backends%20for%20Frontends.png" alt="Backends for Frontends" style="width:100%"/>
</a>
</figure>

*Backends for Frontends* feature a service \(*BFF*\) for each kind of the system’s client\. A *BFF* may be a *Proxy*, *Orchestrator* or both\. Each *BFF* communicates with all the components below it\. The pattern looks like multiple *Proxies* or *Orchestrators* deployed in parallel\.

*<ins>Includes</ins>*: Layered Microservice Architecture\.

### [Service\-Oriented Architecture]({{< relref "../part-4--fragmented-metapatterns/service-oriented-architecture--soa-.md" >}})

<figure style="text-align:center">
<a href="/Contents/Service-Oriented%20Architecture.png" style="outline:none">
<img src="/Contents/Service-Oriented%20Architecture.png" alt="Service-Oriented Architecture" style="width:100%"/>
</a>
</figure>

*SOA* comprises three or four layers of services, with each layer making a domain\. The upper layer contains *Orchestrators* which are often client\-specific, just like *BFFs*\. The second layer incorporates business rules and is divided into business subdomains\. The lower layer\(s\) are libraries and utilities, grouped by functionality and technologies\. Any component may use \(orchestrate\) anything below it\.

*<ins>Includes</ins>*: Segmented Architecture; distributed monolith, enterprise SOA\.

### [Hierarchy]({{< relref "../part-4--fragmented-metapatterns/hierarchy.md" >}})

<figure style="text-align:center">
<a href="/Contents/Hierarchy.png" style="outline:none">
<img src="/Contents/Hierarchy.png" alt="Hierarchy" style="width:100%"/>
</a>
</figure>

Some domains allow for hierarchical composition where the functionality is spread throughout a tree of components\.

*<ins>Includes</ins>*: Orchestrator of Orchestrators, Presentation\-Abstraction\-Control \(PAC\) and Hierarchical Model\-View\-Controller \(HMVC\), Bus of Buses, and Cell\-Based \(Microservice\) Architecture \(WSO2 version\) \(Services of Services\)\.

<nav>

| \<\< [Combined Component]({{< relref "../part-3--extension-metapatterns/combined-component.md" >}}) | ^ [The pattern language of software architecture]({{< relref "../_index.md" >}}) ^ | [Layered Services]({{< relref "../part-4--fragmented-metapatterns/layered-services.md" >}}) \>\> |
| --- | --- | --- |

</nav>



