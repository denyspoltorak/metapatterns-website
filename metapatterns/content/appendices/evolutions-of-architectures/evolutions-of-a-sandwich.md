+++
weight = 18
title = "Evolutions of a Sandwich"
description = "A Sandwich allows for easy changes in its domain layer and can be transformed into Layers or Layered Services."
images = ["/diagrams/Web/og/Favicon-plain.png"]
[sitemap]
  priority = 0.3
+++

# Evolutions of a Sandwich {anchor=false}

Unique evolutions of a [*Sandwich*]({{< relref "../../extension-metapatterns/sandwich.md" >}}) involve the system’s domain logic or its topology:

- The [*domain\-level*]({{< relref "../../basic-metapatterns/layers.md#domain-business-rules-or-model" >}}) *services* are independent enough to be easily added or removed\.
- In most cases they share technologies, allowing for splitting or merging of the services\.
- If the services are found to be strongly coupled, they can be merged into a monolithic layer, likely to be subdivided in a better way later on\.
- Alternatively, the subdomains can be further decoupled\.


## Add or remove a domain\-level service

<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20add%20remove%20Service.png" alt="Sandwich add remove Service" loading="lazy" width="1183" height="243" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Sandwich]({{< relref "../../extension-metapatterns/sandwich.md" >}})\.

<ins>Goal</ins>: maintain effective development\.

<ins>Prerequisite</ins>: a new subdomain emerges or an old one becomes obsolete\.

Though the *Sandwich* architecture allows for subdomains to be pretty independent in their logic, you should update your system’s high\-level structure whenever there are drastic changes in the domain knowledge or functional requirements\. Creation or deletion of a component often means forming or disbanding a team \(see [*Inverse Conway Maneuver*](https://martinfowler.com/bliki/ConwaysLaw.html)\)\.

<ins>Pros</ins>: 

- The system architecture is clear as it follows the domain knowledge\.
- The development teams remain narrowly specialized, thus effective\.
- Dead domain\-level code is easily identified and removed\.


<ins>Cons</ins>: 

- You may need to update your database schema\.
- A newly established team takes time to learn its area of responsibility, while disbanding an old team disrupts almost everyone on the project\.


## Split or merge domain\-level services

<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20split%20merge%20Services.png" alt="Sandwich split merge Services" loading="lazy" width="1240" height="243" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Sandwich]({{< relref "../../extension-metapatterns/sandwich.md" >}})\.

<ins>Goal</ins>: maintain effective development\.

<ins>Prerequisite</ins>: business requirements gradually diverge from your original vision\.

[Ideally]({{< relref "../../foundations-of-software-architecture/modules-and-complexity.md#coupling-and-cohesion" >}}), each service should be kept cohesive, while the services should be decoupled from each other\. However, business likes to mess up your plans\. If you ignore the results, your teams will be slowed down by mutual dependencies or become overburdened by the size of the components which they maintain\. Therefore restructure both the system and teams once the divergence between the domain knowledge and system architecture starts to negatively impact development\.

<ins>Pros</ins>: 

- The system architecture is kept clear as it follows the domain knowledge\.
- System components remain cohesive inside and decoupled from each other\.
- The development teams are narrowly specialized, thus effective\.


<ins>Cons</ins>: 

- You will have to update the database schema and integration logic \(use cases\)\.
- Splitting or merging teams disrupts them\.


## Merge all the domain\-level services together

<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layers.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layers.png" alt="Sandwich to Layers" loading="lazy" width="1104" height="243" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Layers]({{< relref "../../basic-metapatterns/layers.md" >}})\.

<ins>Goal</ins>: improve the system performance and, possibly, development efficiency\.

<ins>Prerequisite</ins>: the project is small but found to be strongly coupled\.

Often the project grows in an unexpected manner\. If you see that the domain\-level services interact intensely, that likely means that you chose a wrong architecture\. Revert to *Layers* to remove the artificial interfaces\. You may also consider merging the domain\-level teams if there are not too many people in them\.

<ins>Pros</ins>: 

- Less indirection and boilerplate code\.
- Improved performance which can be further optimized\.


<ins>Cons</ins>: 

- Now all the teams face higher system complexity\.
- The teams will share the codebase which means a high level of interdependency\.


## Subdivide both shared layers

<figure>
<a href="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png">
<picture>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Evolutions/2/Sandwich%20to%20Layered%20Services.png" alt="Sandwich to Layered Services" loading="lazy" width="1343" height="263" style="width:100%"/>
</picture>
</a>
</figure>

<ins>Patterns</ins>: [Three\-Layered Services]({{< relref "../../fragmented-metapatterns/layered-services.md#orchestrated-three-layered-services" >}}) \([Layered Services]({{< relref "../../fragmented-metapatterns/layered-services.md" >}})\)\.

<ins>Goal</ins>: fine\-grained scalability, database performance optimization, limited fault tolerance\.

<ins>Prerequisite</ins>: the subdomains are loosely coupled in both use cases and data\.

It is natural to divide a *Sandwich* into [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}), but only if your domain is not [data\-centric]({{< relref "../../foundations-of-software-architecture/arranging-communication/programming-and-architectural-paradigms.md#procedural-data-centric-paradigm--shared-data" >}}) \(built around a [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\) and your use cases are not too complex \(requiring an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}})\)\.

<ins>Pros</ins>: 

- Independent scaling and deployment of the services\.
- Database technologies can be chosen on a per service basis\.
- Simpler application and database components\.
- Limited fault tolerance – if one of the services fails, others may still respond to clients\.


<ins>Cons</ins>: 

- Complex use cases are hard to implement or debug\.
- Poor latency for use cases that involve multiple subdomains\.
- Any coupling in the data impairs performance and increases costs\.
- Now you’ll have much more work for your DevOps\.
