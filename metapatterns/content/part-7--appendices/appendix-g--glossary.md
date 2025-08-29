+++
weight = 7
title = "Appendix G. Glossary."
+++

# Appendix G\. Glossary\.

*Abstractness* – the scope of information that a *concept* operates\. Highly abstract *concepts* describe the *system’s* behavior in less words\.

*Action* – an act of a *system* that changes its environment\.

*API* \(*application programming interface*\) – a set of *methods* or *messages* that a *component* exposes to its *clients*\.

*Application* – the most *abstract* layer that usually *integrates components* of a less *abstract* layer\.

*Architectural pattern* – a way to structure a *system* or a part of a *system* to achieve desirable properties \(address a set of *forces*\)\.

*Architectural style* – see *architecture*\.

*Architecture* – the structure of a system\. It comprises *components* and their *interactions*\.

*ASS diagram* – a *structural diagram* with *abstraction*, *subdomain* and *sharding* for coordinates\.

*Asynchronous communication* – the mode of *communication* when the sender of the *request message* does not stop the execution of its *scenario* to wait for the confirmation message\.

*Attack surface* – the amount of *components* and functionality that faces an external network \(potentially exposed to hackers\)\.

*Availability* – the percentage of time that the *system* is operational \(satisfies its *users*\)\.

*Bounded context* – a subset of *requirements* and code that shares a set of *concepts*\. Usually consists of internals of a *component* and *APIs* of all the *components* it uses\.

*Business logic* – the thing that *users* pay for\. It is the heart of the business and is usually the largest part of the *project*\. You cannot buy *business logic*, only *implement* it\. *Business logic* comprises *use cases* and *business rules*\.

*Business rules* – *domain concepts* and their relations\. They make the low\-level half of *business logic*\.

*Choreography* – a kind of *workflow* in which *components* that belong to the same level of *abstractness* cooperate to implement a *use case*\.

*Client* – an external *component* or *system* that makes use of a *component* or *system* in question\.

*Cohesion* – the density of logical connections between entities inside a *component*\.

*Colocated* – running in the same address space \(process\) on the same hardware\.

*Communication* – transfer of data or signals in a *system*\.

*Complexity* – the cognitive load caused by the quantity of entities \(*concepts* or *modules*\) and their relations that a programmer needs to operate\.

*Component* – an encapsulated part of a *system*\. It exposes an *API* to the system's *clients* and/or other *components* of the *system*\.

*Concept* – a notion of an element of a *system's* behavior, usually present in *requirements*\.

*Contract* – the informal rules for the behavior of a *component* expected by its *clients*\.

*Control* – a kind of system that supervises physical entities or external programs\.

*Coupling* – the density of logical connections between *components*\.

*Cross\-cutting concern* – a functionality that should be present in multiple *components*\.

*Debugging* – trying to force the *system* to behave correctly from the user’s point of view\.

*Deployment* – uploading a *component* to the hardware that will execute it\.

*Design* – planning for the best way to write code\.

*Design* – see *architecture*\.

*Design space* – the multitude of possible ways to *design* a given project\.

*Development* – building a *project* for its *users*\. Usually involves intermixed *design*, *implementation*, *debugging* and *testing* phases\.

*Distributed* – spread over multiple computers that *communicate* via a network\.

*Domain* – the whole of knowledge \(including *requirements*\) that is needed to build a *system*\.

*Domain* – the middle layer of a *system* that contains its *business rules*\.

*Event* – a signal that carries some meaning for a *system* or *component*\. *Events* may carry data\.

*Fault tolerance* – the ability of a *system* to remain \(at least partially\) operational if one or more of its *components* fail \(become inaccessible due to a hang, crash or a hardware failure\)\.

*Forces* – expected properties of a *system* \(such as its stability or response time\) which are crucial for the *system* to be built, *deployed* and used successfully\.

*Functional requirements* – the *requirements* that describe *inputs* and *outputs* of a *system*, but not its *performance* or stability\.

*Global use case* – a *use case* that involves most of the *components* of a *system*\. Such *scenarios* are strongly affected by the *system’s* structure\.

*Implementation* – the process of writing code\.

*Implementation* – internals of a *component*\.

*Infrastructure* – the lowest layer of a *system* that provides general\-purpose functionality \(tools\) to its upper layers\.

*Input* – *events* or data that a *system* reacts to\.

*Integration* – see *orchestration*\.

*Integration complexity* – the *complexity* of understanding how individual *components interact* to make a *system*\.

*Interactions* – the kinds and routes of *communication* between *components* of a *system*\.

*Interface* – see *API*\.

*Latency* – the delay between a *system’s* receiving *input* and producing a corresponding *output*\.

*Layers* – *components* of a *system* partitioned by the level of *abstraction*\.

*Messaging* – communication by sending short pieces of data\.

*Method call* – invocation of an *interface* method \(or procedure\) of a *component* by another *component*\.

*Metapattern* – a cluster of *patterns* that have similar *structural diagrams* and address related issues\.

*Module* – a *colocated* \(in\-process\) *component*\.

*Non\-functional requirements* \(*NFR*s\) – see *forces*\.

*Notification* – an *event* that one *component* sends to another *component*\(s\) to inform them of some change\.

*Operational complexity* – see *integration complexity*\.

*Orchestration* – a kind of *workflow* where a single dedicated *component* \(*Orchestrator*\) makes use of \(usually multiple\) less *abstract components*\. *Facade* \[[GoF]({{< relref "../part-7--appendices/appendix-b--books-referenced.md#fsa" >}})\] is a good example\.

*Output* – *actions* or data that a *system* produces\.

*Pattern* – a documented approach \(blueprint\) for solving a recurrent programming issue\.

*Pattern Language* – a set of interrelated *patterns* intended to cover most aspects of *designing systems* in a target *domain*\.

*Performance* – a measure of a *system*’s *throughput*, *latency* and *resource* consumption\.

*Persistent data* – data which survives rebooting the software\.

*Pipeline* – a set of *components* for stepwise *processing* of data\.

*Processing* – transformation of *input* data into *output* data\.

*Project* – the process of making a *system*\.

*Pub/sub \(publish/subscribe\)* – a mode of *communication* when one *component* \(*subscriber*\) receives a subset of *notifications* from another *component* \(*publisher*\)\. It is the *subscriber* that chooses which *notifications* it is interested in\.

*Qualities* – the properties which a *component* or \(sub\)*system* manifests to satisfy *forces*\.

*Real\-time* – a *force* that requires the *system* to respond to incoming *events* immediately\.

*Request/confirmation* – a pair of *messages* between two *components* \(Requestor and Executor\)\. The *request* describes an *action* that the requestor wants the executor to run \(R =\> E\)\. The *confirmation* describes the results of the execution \(R \<= E\)\.

*Requirements* – a set of rules that describe the correct \(expected\) behavior of the *system*\.

*Resources* – CPU, memory, network bandwidth and other stuff that costs money\.

*Scaling* – ability to increase *throughput* of a system by providing it with more *resources*\.

*Scenario* – see *use case*\.

*Service* – a *distributed component*\.

*Services* – *components* of a *system* partitioned by *subdomain*\.

*Sharding* – deploying multiple instances of a *component*\.

*Single point of failure* – a software or hardware *component* which if fails makes the entire *system* non\-operational\. High\-*availability systems* should avoid *single points of failure*\.

*SPI* \(*service provider interface*\) – a set of *methods* or *messages* that a *component* expects to be supported by the *components* it uses\.

*State* – data that a *component* keeps between processing its *inputs*\.

*Structural diagram* – a graphical representation of the structure of a \(sub\-\)*system* that shows *components* and their *interactions*\.

*Stub* – a very simple *implementation* of a *module* that allows other *components* that use it to run without starting the original *module*\. *Stubs* are used to *implement modules* concurrently or *test* them in isolation\.

*Subdomain* – a distinct *cohesive* part of *domain* knowledge\.

*Synchronous communication* – the mode of communication when the *requesting component* waits for results of its *request* to another *component* before continuing to run its *task*\.

*System* – a self\-sufficient set of *communicating components* that were brought together or *implemented* to satisfy its *users* \(by running *use cases*\)\.

*Task* – a high\-level sequence of execution steps\. Similar to *use case* or *scenario*\.

*Team* – a few programmers and testers that work on a *component*\. Teams of more than 5 members lose productivity to communication overhead\.

*Testing* – checking how satisfactorily the *system* behaves\.

*Throughput* – the amount of data a *system* can *process* per unit of time\.

*Use case* – a behavior expected by *system*’s users\. A *system* is *implemented* to run *use cases*\. *Use cases* are the high\-level half of *business logic*\.

*User* – a human that uses a *system* and usually pays well if satisfied with its behavior\.

*Vendor lock\-in* – a pitfall when a *system* relies on an external provider so much that it is impossible to change the provider\. It is similar to falling prey to a monopoly\.

*Workflow* – a sequence of actions \(*messages* or *method calls*\) required to *implement* a *use case*\.

| \<\< [Appendix F\. Format of a metapattern\.]({{< relref "../part-7--appendices/appendix-f--format-of-a-metapattern.md" >}}) | ^ [Part 7\. Appendices]({{< relref "../part-7--appendices/_index.md" >}}) ^ | [Appendix H\. History of changes\.]({{< relref "../part-7--appendices/appendix-h--history-of-changes.md" >}}) \>\> |
| --- | --- | --- |


