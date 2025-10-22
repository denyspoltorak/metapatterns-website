+++
weight = 4
title = "Shared data"
description = "Communication through shared data benefits data-centric domains where multiple services need to operate on a common dataset."
images = ["/diagrams/Web/og/Shared%20data.png"]
[sitemap]
  priority = 0.5
+++

# Shared data {anchor=false}

The final approach is integration through shared data \([*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}})\):

<figure>
<a href="/diagrams/Communication/Services%20to%20Shared%20Data.png">
<picture>
<source srcset="/diagrams/Communication/Services%20to%20Shared%20Data.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Services%20to%20Shared%20Data.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Services%20to%20Shared%20Data.png" alt="Services to Shared Data" loading="lazy" width="1043" height="284" style="width:100%"/>
</picture>
</a>
</figure>

The shared data is a “blackboard” available for each service to read from and write to\. It is passive \(as controlled by the services\) and does not contain any logic except for the data schema, which represents a part of the domain knowledge\. That makes communication through shared data the antipode of [orchestration]({{< relref "../../foundations-of-software-architecture/arranging-communication/orchestration.md" >}}), which also features a shared component, [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), which is, however, active \(controls services\) and contains business logic, not data\.

Shared data can be used for storage, messaging, or both:

## Storage

The most common case of shared data is storage \(usually a database, sometimes a file system\) for a \(sub\)domain that has functionally independent services which operate on a common dataset\. For example, a ticket purchase service and a ticket refund service share a database of ticket details\. The ticket purchase service reads in the available seats and fills in ticket data for purchases\. The ticket refund service should be able to find all tickets bought by a user and delete the user data from seats refunded\. The only communication between the purchase and refund services is the shared database of tickets or seats, so that one of them sees the changes made by the other the next time it reads the data\.

<figure>
<a href="/diagrams/Communication/Purchase%20and%20Return.png">
<picture>
<source srcset="/diagrams/Communication/Purchase%20and%20Return.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Purchase%20and%20Return.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Purchase%20and%20Return.png" alt="Purchase and Return" loading="lazy" width="923" height="323" style="width:100%"/>
</picture>
</a>
</figure>

With this model the services don’t depend on each other – instead, they depend on the shared \(domain\) data format and the database technology\. Thus, it is easy to add, modify, or remove services but hard to change the shared data structure or, especially, the database vendor\.

<figure>
<a href="/diagrams/Communication/Shared%20Data%20-%20Dependencies.png">
<picture>
<source srcset="/diagrams/Communication/Shared%20Data%20-%20Dependencies.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Shared%20Data%20-%20Dependencies.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Shared%20Data%20-%20Dependencies.png" alt="Shared Data - Dependencies" loading="lazy" width="803" height="202" style="width:92%"/>
</picture>
</a>
</figure>

<figure>
<a href="/diagrams/Communication/Shared%20Data%20add%20a%20Service.png">
<picture>
<source srcset="/diagrams/Communication/Shared%20Data%20add%20a%20Service.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Shared%20Data%20add%20a%20Service.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Shared%20Data%20add%20a%20Service.png" alt="Shared Data add a Service" loading="lazy" width="1103" height="203" style="width:100%"/>
</picture>
</a>
</figure>

Services usually need to coordinate their actions\. Commonly, services with a shared database rely on a messaging [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) for communication\. Users of our ticketing system will want to be notified \(through email, SMS or an instant message\) when a free seat that they are interested in appears\. We’re not going to complicate either of the existing services by integration with instant messengers, so we will create a new notification service, which must track each returned ticket to see if any user wants to buy it\. This is easily implemented by the return service publishing and the notification service subscribing to a ticket return event, mixing in a bit of choreography into our data\-centric backend\.

<figure>
<a href="/diagrams/Communication/Notification%20to%20Notification.png">
<picture>
<source srcset="/diagrams/Communication/Notification%20to%20Notification.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Notification%20to%20Notification.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Notification%20to%20Notification.png" alt="Notification to Notification" loading="lazy" width="983" height="323" style="width:100%"/>
</picture>
</a>
</figure>

Another case is found with data processing pipelines where an element may periodically read new files from a folder or new records from a database table to avoid implementing notifications\. This increases latency and may cause a little CPU load when the system is idle, but is perfectly ok for long\-running calculations\.

<figure>
<a href="/diagrams/Communication/Shared%20files.png">
<picture>
<source srcset="/diagrams/Communication/Shared%20files.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Shared%20files.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Shared%20files.png" alt="Shared files" loading="lazy" width="1016" height="423" style="width:100%"/>
</picture>
</a>
</figure>

Finally, there is the rarely used option of an external [*Scheduler*]({{< relref "../../extension-metapatterns/proxy.md#load-balancer-sharding-proxy-cell-router-messaging-grid-scheduler" >}}) which selects the services which should run based on the data available\. This is known as the [*Blackboard* pattern]({{< relref "../../extension-metapatterns/shared-repository.md#blackboard" >}}) and [something similar]({{< relref "../../basic-metapatterns/monolith.md#inexact-reactor-with-extractors-phased-processing" >}}) happens in 3D game engines\. The *Scheduler* \(which is an [*Orchestrator*]({{< relref "../../extension-metapatterns/orchestrator.md" >}}), by the way\) is needed when CPU \(or GPU or RAM\) resources are much lower than what the services would consume if all of them ran in parallel, thus they must be given priorities, and the priorities change based on the context which is regularly estimated from the system’s data\.

<figure>
<a href="/diagrams/Communication/Blackboard.png">
<picture>
<source srcset="/diagrams/Communication/Blackboard.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Blackboard.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Blackboard.png" alt="Blackboard" loading="lazy" width="803" height="283" style="width:100%"/>
</picture>
</a>
</figure>

## Messaging

The other, not as obvious, use case for shared data is messaging, which is implemented by the sender writing to a \(shared\) queue \(or log\) while the recipient is waiting to read from it\. Queues can be used for any kind of messages: request/confirm pairs, commands, or notifications\. Each service may have a dedicated queue \(either input for commands mode or output for notifications\), a pair of queues \(messages from the service’s output are duplicated by an underlying distributed [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}}) to input queues of their destinations\), or there may be a queue per communication channel, or a single queue for the entire system \(or a global queue per message priority\) with each message carrying destination id \(for commands\) or topic \(for notifications\)\.

<figure>
<a href="/diagrams/Communication/Queues.png">
<picture>
<source srcset="/diagrams/Communication/Queues.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Queues.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Queues.png" alt="Queues" loading="lazy" width="803" height="603" style="width:100%"/>
</picture>
</a>
</figure>

The use of shared data for messaging turns our datastore into a [*Middleware*]({{< relref "../../extension-metapatterns/middleware.md" >}})\. The dependencies are identical to [those in choreography]({{< relref "../../foundations-of-software-architecture/arranging-communication/choreography.md#dependencies" >}}) – each service depends on the APIs of its destinations for commands or its sources for notifications\.

There should be a means for the recipient of a message to know about its arrival so that it starts processing the input\. Usually a messaging *Middleware* implements a receive\(\) method for the service to block on\. However, very low latency applications, like [HFT](https://en.wikipedia.org/wiki/High-frequency_trading), may [busy\-wait](https://en.wikipedia.org/wiki/Busy_waiting) by repeatedly re\-reading the shared memory so that the service starts processing the incoming data immediately on its arrival, bypassing the OS scheduler\. This is the fastest means of communication available in software\.

## Full\-featured

Finally, some \(usually distributed\) datastores implement data change notifications\. That allows for the services to communicate through the datastore in real\-time, removing both the need for an additional *Middleware* and interdependencies for the services\. Such a system follows the [*Shared Repository*]({{< relref "../../extension-metapatterns/shared-repository.md" >}}) pattern of \[[POSA4]({{< relref "../../appendices/books-referenced.md#posa4" >}})\] which was rectified as [*Space\-Based Architecture*]({{< relref "../../extension-metapatterns/shared-repository.md#data-grid-of-space-based-architecture-sba-replicated-cache-distributed-cache" >}}) \[[SAP]({{< relref "../../appendices/books-referenced.md#sap" >}}), [FSA]({{< relref "../../appendices/books-referenced.md#fsa" >}})\]\. In our example, the available seats notification service subscribes to changes in the seats data in the database – this way it does not need to be aware of the existence of other services at all\. We can also move the email notifications logic of the ticket purchase service into a separate component which would track purchases in the database and send a printable version of each newly acquired ticket to the buyer’s email address which can be found in the ticket details in the database\.

<figure>
<a href="/diagrams/Communication/Notification%20inside%20the%20DB.png">
<picture>
<source srcset="/diagrams/Communication/Notification%20inside%20the%20DB.svg" media="(prefers-color-scheme: light)"/>
<source srcset="/diagrams/Communication/Notification%20inside%20the%20DB.dark.svg" media="(prefers-color-scheme: dark)"/>
<img src="/diagrams/Communication/Notification%20inside%20the%20DB.png" alt="Notification inside the DB" loading="lazy" width="1123" height="323" style="width:100%"/>
</picture>
</a>
</figure>

## Summary

Communication through shared data is best suited for data\-centric domains \(for example, ticket purchase\)\. It allows for the services to be unaware of each other’s existence, just as they are with orchestration, but the structure of the domain data becomes hard to change as it is referenced all over the code\. Shared data may also be used to implement messaging\.