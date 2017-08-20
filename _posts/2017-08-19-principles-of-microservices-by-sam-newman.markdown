---
layout: post
title:  "Principles of Microservices by Sam Newman"
date:   2017-08-19 13:00:00 +0100
categories: jekyll update
---

[Principles of Microservices](https://www.youtube.com/watch?v=PFQnNFe27kU)

Microservices are small **autonomous** services that **work together**, modeled around a **bussiness domain**.

They are about independent evolution, each service is independently releasable.  You can make a change and deploy them into production on their own.

## The Principles

1. **Model around a business domain**, provides stable APIs.
2. **Decentralize**, allows agility.
3. **Embrace automation**, allows scalability at speed.
4. **Hide implementation details**, allows evolution.
5. **Deploy independently**, allows agility and availability.
6. **Consumer first**, provides useful services.
7. **Isolate failure**, allows availability.
8. **Highly observable**, provides maintainability.


### Model around a business domain

* You should be able to understand the domain when looking at the architecture.

* Services should represent a bounded context within the domain.

* Individual services should not be technical layers (e.g. you should not have a data layer service etc) as changes generally span all layers and changes that cross service boundaries are expensive.

* [Hexagonal architecture](http://alistair.cockburn.us/Hexagonal+architecture) is good for individual services.


### Decentralize

* Both decision making and design concepts.

* Services are owned and operated by a team.

* Autonomy is giving people as much freedom as possible to do the job at hand.

* Governance you do not have a centralized architect this is decided by members of the team or across teams for wider concerns:
  * Shared communities and practices help promote organizational consistency and learning.

* Actions that should be in the control of the team are:
  * Provisioning of development environment.
  * Provisioning and deploying infrastructure.
  * Deploying service to any/all environments.
  * Management of services in any/all environments.
  * Tearing down services when no longer needed.
  * Architecture and design decisions.

* Communication should be dumb:
  * Messaging.
  * Api gateway.


### Embrace automation

* It is needed because you have a lot more complexity in your environment.

* To allow working at scale and support the *Decentralization* principle automation should be pursued relentlessly.

* The speed at which new services can be commissioned will increase over time.  The initial cost is in time invested in automation.

* Common growth rates:
  *  3 Months, 2 services.
  * 12 Months, 6 services.
  * 18 Months, 60 services.
  * Initial cost of automation is high.

* Items that should be automated:
  * Project commissioning.
  * Infrastructure provisioning/deployment.
  * Build.
  * Test:
    * Unit/Functional.
    * Security.
    * Performance.
    * Contract.
  * Deployment.
  * Teardown.

* Requires a mindset of every commit being a release candidate.


### Hide implementation details
* Services must be able to evolve independently of each other.
* Data is shared via an API **not** a database.
* Services have their own concept of domain entities.
* Instance identity is common across all services.  e.g. A specific customer should have the same id across all services even though the model of a customer differs in each service.
* DTO that pass between services are not the internal domain model.
* Only expose the minimum data you need to.  It is easier to expose hidden data than it is to hide exposed data.


### Deploy independently

* You should be able to change a service and deploy it without having to make a change to any other service.

* You should be able to deploy a service without affecting any other service.

* One service per host:
  * A host is a virtual machine or container.
  * Requires cheap provisioning of infrastructure.
  * Multiple services per host means side effects and dependencies.
  * Docker makes this easy.

* Making changes:
  * Key question is "Have I broken one of my consumers?"
  * Consumer driven contracts:
    * Consumer expectations are written in executable tests.
    * Tests are written by the consumer.
    * Tests are run as part of the service's build pipeline.
    * There are tool to support contract testing:
        * [Pact](https://docs.pact.io/).
  * When you need to break consumers:
    * Need to give the consumer time to change.
    * Co-existing endpoint:
      * A breaking API change results in a new endpoint.
      * Old endpoints will be retired but gives consumers time.
  * When you can not break the contract:
    * Brand new version of the service.
    * Increases maintenance cost.


### Consumer first

* Services exist to be called so they should be built outside in rather than inside out.
* An API is a user interface.
* An API should be documented:
  * [Swagger](https://swagger.io/).
* Make it easy to understand what services there are:
  * Service discovery ([Consul](https://www.consul.io/))
  * Context maps.


### Isolate failure

* Failure in one service should not have a cascading effect.
* Distributed systems increase the risk of failure.
* Slow failure ties up resources.
* Timeouts for requests should be small:
  * Based on percentile response times (90th percentile).
* Bulkheads, resources needed to access individual services are pooled on a per service basis:
  * One bulkhead per service that you depend on.
  * Prevents one service failure bringing down your entire system.
* Circuit breakers, After a certain number or request failures you stop sending requests.
    * Allow you to fail fast.
    * Allows planned outage when deploying.
    * [Polly](https://github.com/App-vNext/Polly) .net circuit breaker.


### Highly observable

* Making sure it is easy to understand how things hang together and how they behave.

* Operating behavior:
  * Aggregate logs.
  * Profile normal operation:
    * Response rates

* Understand behavior:
  * Correlation id.
  * Include in logs.
  * Visualization maps can be built from aggregated logs.
