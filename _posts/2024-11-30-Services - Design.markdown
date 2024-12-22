---
title: "Services - Design"
date: 2024-11-30 9:15:43
categories: [services]
tags: [services]
---

When designing services, it is recommended to use a Domain Driven Design (DDD) approach, which is a software engineering method used to solve complex domain models.

DDD emphasizes the business model by aligning software implementation with core business concepts. It employs a standardized vocabulary that includes terms such as domain logic, subdomains, bounded contexts, context maps, and domain models.

A ubiquitous language is also required to facilitate collaboration between business/domain SMEs and development teams. In order to leverage a DDD model for service design, it is important to adopt the following approach to define the appropriate domain models and application services:

<h4>Domain Analysis</h4>
The term "domain" refers to the business aspects of the solutions, which will generate the requirements and use cases to be implemented by the software. A domain may encompass various business areas, and a clear separation of concerns should be established between services using boundaries for successful service implementation.

The outcome of a domain analysis exercise is a domain model that captures the essential concepts, processes, and ubiquitous language for the business domain, which requires a thorough understanding of the business. A domain model can comprise several subdomains representing distinct business areas.

<h4>Bounded Context</h4>
Once the domain model is defined, the subsequent step is to establish the bounded contexts. This pattern is fundamental to DDD and aids in dividing a large domain model into distinct context boundaries with well-defined relationships between them. In each boundary, all terms and phrases of the ubiquitous language have specific meanings, and the model reflects the language. Bounded contexts also explicitly define relationships with other bounded contexts and formalize the interactions between them.

Bounded context can be used as a basis to scope microservices functionality

<ul>
<li>At least one or multiple microservices per bounded context</li>
<li>A code repository per bounded context</li>
<li>One datastore per bounded context</li>
</ul>

<h4>Implementation building blocks</h4>
<h5>Entity</h5>
<h5>Value object</h5>
<h5>Aggregate</h5>
<h5>Service</h5>
<h5>CQRS</h5>
<h5>Events/Messages</h5>
<h5>Event Handlers</h5>
<h5>Message Consumers</h5>
<h5>Factory</h5>
<h5>Repository</h5>

<h4>Microservices</h4>
Each microservice will implement one business responsibility from the bounded context. Services communicate with each other using HTTP REST interface or asynchronous models like event driven architecture.
