---
title: The Scales Aggregates
date: 2017-03-08
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - Scales
  - "Bounded Context"
  - Domain
  - "Event Sourcing"
  - Aggregate
  - Entity
  - "Value Object"
  - "Ubiquitous Language"
---

In the [*Event Storming Anita's scenarios* article](/2017/02/21/Event-Storming-Anitas-scenarios/), we identified several *Entities* out of our *Event Storming session* for *Anita*, our *Primary Persona*.

And in the [previous article][] we reviewed the [general rules about *Aggregates*](/2017/02/26/Designing-Aggregates/#General-rules-about-Aggregates) as well as [a framework for designing our *Aggregates*](/2017/02/26/Designing-Aggregates/#A-framework-for-designing-our-Aggregates). 

Let's now apply these recommendations to our [Scales][] business model.

[previous article]: /2017/02/26/Designing-Aggregates/
[Scales]: /products/scales

<!-- more -->



## A note about identity management

I am used to trivial identity management using the underlying persistence store as in typical *CRUD* applications. In several [DDD][] examples however, I have seen identities designed as *Value Objects* rather than a numeric property of the *Entity*, for example:
{% plantuml %}
    class MyEntity <<(E, lightgreen) Aggregate Root>>
	class MyEntityId <<(V, lightblue) Value Object>>
	MyEntity -> MyEntityId
{% endplantuml %}

I (bought and) digged into the [Implementing Domain-Driven Design][iddd-book] book about this specific topic. As the [DDD][] approach looks towards the design of integrated systems, identity management may become more challenging when considered across several *Bounded Contexts*, especially when using *Event Sourcing*.

I won't discuss this topic here. For now, we should simply keep in mind that identities should be implemented as *Value Objects* distinct from their entity.

[DDD]: https://en.wikipedia.org/wiki/Domain-driven_design
[iddd-book]: http://amzn.com/0321834577



## Should we create a formal document for our *Ubiquitous Language*?

As previously stated, the *Ubiquitous Language* is the shared and fully agreed vocabulary among the team, including both business and technical stakeholders. It is circumvented to the *Bounded Context* and its definitions must be perfectly clear inside it.

A concept inside the *Ubiquitous Language* includes obviously its definition as a textual description, but it also includes all the business rules that describes how it behaves and interact with other concepts and the environment.

While it is a good starting point to use textual description and visual diagrams to explicit the *Ubiquitous Language*, it is ultimately the running code that defines the actual implementation of this language, and this implementation is living and evolving. Maintaining formal documentation in sync with the changes in code is quite expensive, so we have to find a good balance.

In our series, we will keep formal definitions quite minimalistic, and focus on writing **testable and meaningful source code** as the living implementation of our domain model. Diagrams will also be used as a transient, visual mean of communicating our design.


## First step: Create one *Aggregate* for every *Entity*

We previously identified the following *Entities*:
- *Indicator*
- *Scale*
- *Indicator Instance*
- *Scale Instance*

We will start by creating one *Aggregate* for every one of them. Designing *Aggregates* requires some careful thinking. I would like to describe with enough detail the inner steps of that design process, I will create one article for every *Aggregate* in the following articles.

Let's start with [the *Indicator* *Aggregate*](/2017/03/13/The-Indicator-Aggregate/).