{% include navbar.html %}


# Principles

[Keep it simple](#keep-it-simple)

[Emergent design over upfront design](#emergent-design-over-upfront-design)

[High-level long-term options over detailed long-term design](#High-level-long-term-options-over-detailed-long-term-design)

[Shared services over shared code](#Shared-services-over-shared-code)

[Know the limitations of your architecture](#Know-the-limitations-of-your-architecture)

[Microservices over monoliths](#Microservices-over-monoliths)

[Composability over all-in-one systems](#Composability-over-all-in-one-systems)

[Cooperative architecture over siloed architecture](#Cooperative-architecture-over-siloed-architecture)

[Data services over databases](#Data-services-over-databases)

[Boundaries of responsibility over shared ownership](#Boundaries-of-responsibility-over-shared-ownership)

[REST over RPC](#REST-over-RPC)

[Requesting data over sending data](#Requesting-data-over-sending-data)

[Dumb pipes and intelligent endpoints over middleware](#Dumb-pipes-and-intelligent-endpoints-over-middleware)

[Presume service failure over trusting service availability](#Presume-service-failure-over-trusting-service-availability)

[Open-source over closed-source](#Open-source-over-closed-source)

[Open Standards over Proprietary Standards](#Open-Standards-over-ProprietaryStandards)

[Modern browser interfaces by default](#Modern-browser-interfaces-by-default)

[Public cloud by default](#Public-cloud-by-default)

[Continuous delivery over change management](#Continuous-delivery-over-change-management)

[Trust but verify code](#Trust-but-verify-code)

[Automation over manual intervention](#Automation-over-manual-intervention)

[Secure systems over secure zones](#Secure-systems-over-secure-zones)

## Keep it simple

![alt text](https://github.com/lanejes/Architecture-Principles-for-Justice/raw/master/images/A,B,C.png "Simplicity")

Software systems are never ‘done’. We should always be in a position to update and improve any information system in justice. If we are able to change systems, we negate the advantages of so-called ‘future-proofing’: adding features to anticipate the result of decisions that have not yet been made. 

Keep it simple and avoid future-proofing, as it adds avoidable complexity to a system which has undesirable side-effects:

* Higher chance of system failure
* Longer & more expensive fixes and investigations on failure
* Harder to understand the system
* Reduces speed of change 
* Raises the cost of change

## Emergent design over upfront design

We should expect architectural design to adapt over time as systems are built. Delivery team members, such as developers, should end up having significant influence on the design of systems as they are much closer to the problem space. High level designs, often provided by architects, should guide and provoke debate, but never dictate. 

## High-level long-term options over detailed long-term design

Designs for systems which will be built over a long-period of time should not be detailed. Detailed long-term designs have a very low chance of being accurate due to the unpredictable nature of delivery and technology. If they are unlikely to be accurate they have the disadvantages of:

* Setting false expectations to stakeholders
* Disempowering developers and other delivery professionals who are the most likely , and most informed people to challenge the detailed design
* Wasting money on analysis and documentation

However, several areas remain vital for long term architectural planning:

* Establishing high-level principles (where these principles are not sufficient)
* Establishing the key datasets, and determining ownership/stewardship
* Identifying existing systems or services which may help solve the problem
* Ensuring clashes of scope are avoided with other long term architectural options
* Sketching the likely system purposes, and connections at a high level
* Sharing the plan so that other can help spot the emerging patterns and overlaps with their work

## Shared services over shared code

Code libraries are difficult to share well: they contribute to the complexity and risks of the systems which use them. Conversely, simple services with well-defined boundaries can be used as a black box, so long as there is confidence in how the service will be provided. Where both code library and service seem like viable options, the service options should be taken.

## Know the limitations of your architecture

For any production system, we should be clear what its limitations are. We should not simply demonstrate that it can handle a predicted volume and shape of usage. Instead, we should test systems to breaking point to ensure we understand the system, and to ensure that our tests are effective. 

## Microservices over monoliths

We define microservices as:

* Small: Replaceable with like-for-like features, but new technologies in less than 12 weeks
* Encapsulating domain concepts, or interaction patterns (back-ends for front-ends)
* Separated by the simplest protocols, generally REST over HTTP

We define monoliths as:

* Large: Replaced with like-for-like features, but new technologies in over 6 months
* Encapsulating an entire domain, or a large section of a domain
* Internally coupled by a variety of protocols

Microservices are considered generally better than monoliths, but the tradeoffs must be considered:

Costs

* More defensive code and architecture
* More marshalling/unmarshalling (transforming data from a flat structure, such as JSON, into programming-language-specific data structure, and vice versa)
* Eventual consistency issues and additional complexity
* Some failures have complex causality

Benefits

* Ease of change
* De-duplication (lower cost)
* Independent deployment
* Technology diversity
* Quicker tests & deployment
* Localised failure
* Flexible organisation design 
* Clearer boundaries of ownership

It should be noted that a common pattern for the development of new systems is for microservices to emerge only once the design becomes clearer understood, meaning that a **monolithic architecture will exist during the early stages.**

## Composability over all-in-one systems

Many existing systems are all-in-one: they seek to solve the problems of a localised area of an organisation, including more general concerns shared by the directorate, department, government or commercial sector. This principle encourages these systems to be composed of many parts, which are divided according the microservice principle.

This is a challenging principle, because many of these parts do not yet exist. However, we might expect a ‘case management system’ of the future to be composed of, for example:

* An already available suite of Platform-as-a-Service tools (e.g. monitoring)
* An already available staff identity service
* An already available payments handling service
* An already available ‘toolkit’ of UI components
etc

## Cooperative architecture over siloed architecture

This principle builds on the idea of microservices, but requires architects to look beyond the boundaries of their programme/agency/department/government and find others to cooperate with. Who else is solving these problems? Can they provide parts of the solution.

* Other agencies or programmes (HMCTS, LAA, NOMS)
* Departmental centres (MOJ Digital & Technology)
* Government centres (CESG, GDS)
* Other departments (HMRC, Home Office)
* The commercial sector (Software-as-a-service products, or open-source solutions)

## Data services over databases
Identify authoritative data sets and build **registers** as data services around them. Build platforms and services for users from these fundamental building blocks. The data services are your foundations and don't change very often, whereas services for users will change more frequently.

## Boundaries

Having too many system owners can result in disagreements over direction, and subsequent stagnation. When ownership of a single system is unclear, it is recommended to use the data principles to find the boundaries within the system, and to create a clean, REST API separation between different areas of data stewardship. Transfer of system ownership should follow this separation.

## REST over RPC

**RESTful consumption** of APIs provides the most effective form of decoupling between two connected systems - allowing them to change independently. REST patterns layer upon decades-established technologies, and are consistently supported by code frameworks. Other approaches cannot provide the same level of decoupling, and are encumbered by a lack of widespread support. 

## Avoid breaking changes

Where two services connect, various techniques should be used by both client and service to prevent breaking changes. REST, and its **HATEOAS constraint**, provides a good framework for this, but also **semantic API versioning** and **tolerant readers**.

## Requesting data over sending data

Where services connect, the majority of data, and the majority of interactions involve requesting data. In REST terms, this means that the majority of interactions are GET requests.

Where it is necessary to transmit a notification of an event, the event data should be kept to a minimum. More data can be requested in response to receiving a notification.

This approach allows for clearer boundaries between services and organisations. It facilitates lower cost architecture which can leverage caching, and avoid duplication of storage. 

Caching techniques can be used to make systems more efficient, appropriate techniques should be used to ensure that caches act as caches:

* Caches are invalidated appropriately to keep the data up-to-date
* There is a clear understanding from systems using the cache that the data may not be 100% consistent with the source data
* Systems should be capable of full recovery from an empty cache
* Caches should be avoided if the source data is sufficiently available

It is acknowledged that some high-data-throughput scenarios may negate this principle, but also that these represent <0.1% of government data sharing.

## Dumb pipes and intelligent endpoints over middleware

The pattern of ‘messaging middleware’ should be avoided. In particular, it is discouraged at organisational boundaries. Adhering to the previous principle prevents the need for intermediate systems focused only on messages, or processing data streams. 

This approach leads to an architecture of intelligent software systems with simple and minimal connections between them peer-to-peer.

The changes of technology over the last 10 years, in particular IaaS and deployment automation, has made architectures comprising thousands of small interconnected systems possible and desirable.

It is acknowledged that some high-data-throughput scenarios may negate this principle, but also that these represent <0.1% of government data sharing.

## Presume service failure over trusting service availability

It should be assumed that services, and the networks on which they reside, will fail. Services should protect against this scenario through various defensive coding techniques **such as timeouts, circuit breakers and bulkheads.**

## Open-source over closed-source

Where software: 

1. Is incorporated into government-authored software (e.g. code libraries) 
2. Is installed as a package on government-run server operating systems or containers

Open source should be used.

Where software:

3. Powers commercial software-as-a-service, platform-as-a-service or infrastructure-as-a-service
Runs on end-user devices

4. Open source should be considered an advantage, but not necessary.

## Open Standards over Proprietary Standards

Proprietary standards promote lock in, normally to the vendor that created them. This reduces choice, stifles innovation, and often raises the cost of future change.

**Open standards** enable interoperability and encourage innovation. They also encourage cooperative approaches to architecture across government, and across the industry.

## Modern browser interfaces by default

The user interfaces presented to all types of users, from citizens through to civil servants, will be provided through modern browsers. All market leading browsers now update automatically, meaning that this is a process of continuous iteration, and choice of technology which limits the impact of browser updates.

## Public cloud by default

Public cloud, provided at scale by commercial technology companies has emerged as the lowest risk environment for our information systems to reside. We should take this option by default unless it can be demonstrated that a more bespoke privately hosted environment can offer specialised characteristics, without overly compromising the characteristics offered by public cloud such as low unit cost, high availability and rapid provisioning. 
Optimise for change

## Continuous delivery over change management

Build the expectation of regular updates (on a scale of minutes, not days or weeks) into your release process to ensure that changes are repeatable, reliable, and fast. Only by releasing often can you become good at releases, and make them low risk. By having long change processes to follow, the team (and service) become change-averse, and change becomes difficult and dangerous.

## Trust but verify code
To support continuous delivery, the code produced by developers in teams should be trusted. It should be trusted to be published in a public code repository, and it should be trusted to go into production. Trust will be obtained by allowing teams to create the structures and processes internally to ensure the code is high quality.

We must periodically, and systematically verify team approaches to ensuring code quality, but this should never form part of a regular cycle of delivery. 

## Automation over manual intervention

Any process that requires a human is a process that will be missed or done wrong, so repeated tasks should be automated to ensure they are completed correctly, on time, every time. This is doubly true for things that only happen infrequently: humans will do it badly because they are not practised, or because it is incorrectly documented.

Special mention should be given to regularly testing all systems and automations. The only way to be sure something works is to do so regularly. Prove the service you are running is still working by smoke testing it every hour, for example.

## Secure systems over secure zones
Each system will be secured independently of the networks it connected to. This is intended to avoid a coupling between the security features of a system with its network, preventing independent change, and reducing need for expensive controls to address the highest-risk information within a large network. It will also allow for more flexible use of untrusted public networks. 

Restricted or trusted networks can be used, but the security of the system should not rely on the security features of the network. 
