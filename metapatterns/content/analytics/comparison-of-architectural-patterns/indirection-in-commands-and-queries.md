+++
weight = 4
title = "Indirection in commands and queries"
description = "Indirection is implemented with Anticorruption Layer or Open Host Service in OLTP and with CQRS View or Reporting Database in OLAP systems."
images = ["/diagrams/Conclusion/Indirection-Command.png"]
[sitemap]
  priority = 0.5
+++

# Indirection in commands and queries

*We can solve any problem by introducing an extra level of indirection* – states the [old adage](https://en.wikipedia.org/wiki/Fundamental_theorem_of_software_engineering)\. We will not explain how this rule drives [deep learning](https://en.wikipedia.org/wiki/Deep_learning), at least for now\. Instead, let’s concentrate our effort on indirection in communication between services\.

Each component operates its own *domain model* \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] which translates into objects and/or procedures convenient for use in the component’s subdomain\. However, should a system cover multiple subdomains, the best models for its parts to operate start to mismatch\. Furthermore, they are likely to diverge progressively over time as requirements heap up and [the project matures]({{< relref "../../analytics/architecture-and-product-life-cycle.md" >}})\.

If we want for each module or service to continue with a model that fits its needs, we have to protect it from the influence of models of other components by employing indirection – a translator – between them\.

<aside>

> In a system of subdomain\-dedicated [*Services*]({{< relref "../../basic-metapatterns/services.md" >}}) a service may need to operate entities that are defined in another service’s subdomain\. For example, the financial and recruiting departments’ software operates employees, but the employee data which each department needs is different\. Moreover, it also differs from the employee records in the HR department which is responsible for adding, editing, and discarding the employees\. We don’t want our accountants to spend their nights seeking the correlation between salaries, birthday horoscopes from HRs, and MBTI tests from the recruiters\.

</aside>

## Command \(OLTP\) systems

<figure>
<a href="/diagrams/Conclusion/Indirection-Command.png" style="outline:none">
<img src="/diagrams/Conclusion/Indirection-Command.png" alt="Indirection-Command" loading="lazy" width="1995" height="819" style="width:100%"/>
</a>
</figure>

More often than not our system consists of services that command each other: via [RPC](https://en.wikipedia.org/wiki/Remote_procedure_call)s, requests or even notifications – no matter how, one component makes a call to action which other\(s\) should obey\.

In such a case we employ an [*Adapter*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) between two services or an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) when cooperation of several services is needed to execute our command:

- An [*Anticorruption Layer*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] is an *Adapter* on the dependent service’s side: as soon as we call another service, we start depending on its interface, while it is in our interest to isolate ourselves from its peculiarities and possible future changes\. Thus we should better write and maintain a component to translate the foreign interface, defined in terms of the foreign domain model, into terms convenient for use with our code\. Even if we subscribe to notifications, we may also want to have an *Adapter* to transform their payload\.
- An [*Open Host Service*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) \[[DDD]({{< relref "../../appendices/books-referenced.md#ddd" >}})\] resides on the other side of the connection – it is an *Adapter* that a service provider installs to hide the implementation details of its service from its users\. It will typically translate from the provider’s domain model into a more generic \(subdomain\-agnostic\) interface suitable for use by services that implement other subdomains\.
- An [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) \(which can be an [*API Composer*]({{< relref "../../extension-metapatterns/orchestrator.md#api-composer-remote-facade-gateway-aggregation-composed-message-processor-scatter-gather-mapreduce" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\], [*Process Manager*]({{< relref "../../extension-metapatterns/orchestrator.md#process-manager-orchestrator" >}}) \[[EIP]({{< relref "../../appendices/books-referenced.md#eip" >}}), [LDDD]({{< relref "../../appendices/books-referenced.md#lddd" >}})\], or [*Saga Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md#orchestrated-saga-saga-orchestrator-saga-execution-component-transaction-script-coordinator" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\]\) spreads a command to multiple services, waits for each of them to execute their part, and cleans up after possible failures\. It tends to be more complex than other translators because of the coordination logic involved\.


## Query \(OLAP\) systems

<figure>
<a href="/diagrams/Conclusion/Indirection-Query.png" style="outline:none">
<img src="/diagrams/Conclusion/Indirection-Query.png" alt="Indirection-Query" loading="lazy" width="1995" height="784" style="width:100%"/>
</a>
</figure>

There is often another aspect of communication in a system, namely, information collection and analysis\. And it runs into a different set of issues which cannot be helped by mere interface translation\.

Each service operates and stores data in its own format and schema which matches its *domain model*, as discussed above\. When another service needs to analyze the foreign data according to its own domain model, it encounters the fact that the foreign format\(s\) and schema\(s\) don’t allow for efficient processing – in the worst case it would have to read and re\-process the entire foreign service’s dataset to execute a query\.

The solution employs an intermediate database as a translator from the provider’s to consumer’s preferred data access mode, format, and schema:

- A [*CQRS View*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] resides in the data consumer and aggregates the stream of changes published by the data provider\. This way the consumer can know whatever it needs about the state of the provider without making an interservice call\.
- [*Data Mesh*]({{< relref "../../basic-metapatterns/pipeline.md#data-mesh" >}}) \[[LDDD]({{< relref "../../appendices/books-referenced.md#lddd" >}})\] is about each service exposing a general\-use public interface for streaming and/or querying its data\. Maintaining one often requires the service to set up an internal [*Reporting Database*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}})\.
- A [*Query Service*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) \[[MP]({{< relref "../../appendices/books-referenced.md#mp" >}})\] aggregates streams from multiple services to collect their data together, making it available for efficient queries \(joins\)\.


## Summary

We see that though command\-dominated \(*operational* or *transactional*\) and query\-dominated \(*analytical*\) systems differ in their problems, the architectural solutions which they employ to decouple their component services match perfectly:

- [*Anticorruption Layer*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) or [*CQRS View*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) is used on the consumer’s side,
- [*Open Host Service*]({{< relref "../../extension-metapatterns/proxy.md#adapter-anticorruption-layer-open-host-service-gateway-message-translator-api-service-cell-gateway-inexact-backend-for-frontend-hardware-abstraction-layer-hal-operating-system-abstraction-layer-osal-platform-abstraction-layer-pal-database-abstraction-layer-dbal-or-dal-database-access-layer-data-mapper-repository" >}}) or [*Data Mesh*]({{< relref "../../basic-metapatterns/pipeline.md#data-mesh" >}})’s [*Reporting Database*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#reporting-database-cqrs-view-database-event-sourced-view-source-aligned-native-data-product-quantum-dpq-of-data-mesh" >}}) is on the provider’s side,
- [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}) or [*Query Service*]({{< relref "../../fragmented-metapatterns/polyglot-persistence.md#query-service-front-controller-data-warehouse-data-lake-aggregate-data-product-quantum-dpq-of-data-mesh" >}}) coordinates multiple providers\.


Which shows that the principles of software architecture are deeper than the [CQRS](https://en.wikipedia.org/wiki/Command_Query_Responsibility_Segregation) dichotomy\.

<nav>

| \<\< [Dependency inversion in architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/dependency-inversion-in-architectural-patterns.md" >}}) | ^ [Comparison of architectural patterns]({{< relref "../../analytics/comparison-of-architectural-patterns/_index.md" >}}) ^ | [Ambiguous patterns]({{< relref "../../analytics/ambiguous-patterns.md" >}}) \>\> |
| --- | --- | --- |

</nav>