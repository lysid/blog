---
title: 'Digging into core DDD concepts'
date: 2017-02-22
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - Scales
  - "Bounded Context"
  - Domain
  - Subdomain
  - "Context Mapping"
  - Aggregate
  - Entity
  - "Value Object"
  - "Domain Event"
  - Command
---

We are still new to the core [DDD][Domain-Driven Design] concepts, so let's dig into them a little bit here. Again, I'm quoting from the *[Domain Driven Design Distilled][dddd-book]* by [Vaughn Vernon][] (check out my [summary PDF][dddd-summary-pdf] for a quick reference).

[Domain-Driven Design]: https://en.wikipedia.org/wiki/Domain-driven_design
[Vaughn Vernon]: http://vaughnvernon.co
[dddd-book]: http://amzn.com/0134434420
[dddd-summary-pdf]: /assets/DDD-Distilled-Summary.pdf

<!-- more -->



## Bounded Context

We introduced the *Ubiquitous Language* concept in the [previous article](/2017/02/21/Event-Storming-Anitas-scenarios/#Our-Ubiquitous-Language). That shared language is belonging to a single *Bounded context*:

> A *Bounded Context* is a semantic contextual boundary. The components inside a *Bounded Context* are context specific and semantically motivated. You could think of it as part of your problem space.
> 
> Within that *Bounded Context*, the language is called the *Ubiquitous Language* because it is both spoken among the team members and implemented in the software model.
>
> It’s possible that people from other teams would have a different meaning for the same terminology, because their business knowledge is within a different context; they are developing a different *Bounded Context*. Any components outside the context are not expected to adhere to the same definitions.

Some technical considerations are also proposed:

> A *Bounded Context* is where a model is implemented, and you will have separate software artifacts for each *Bounded Context*. 
>
> There should be one team assigned to work on one *Bounded Context*. There should also be a separate source code repository for each *Bounded Context*.
>
> Your team owns the source code and the database and defines the official interfaces through which your *Bounded Context* must be used.

So, simply stated a *Bounded Context* is the conceptual delimitation of our product, including its core business concepts, its terminology and its technical assets.

### A word on *Domain* and *Subdomains*

We won't speak very much about those higher-level concepts in these blog series, since they are targeting strategic company-wide vision. Simply keep in mind that in our case we could say that our *Core Domain* could be a *Patient Information System*, in which [Scales][] is one of the several *Subdomains* that belong to that *Core Domain*.

We can assume that transversal concerns like user authentication, patient identification, etc. will be shared by our core *Subdomains*. We could consider buying off-the-shelf solutions for those *Generic Subdomains*.

[Scales]: /products/scales

### Context Mapping

So, according to these definitions, our *Subdomain* is a "blackbox" for other *Subdomains*. This is reciprocally true of *Generic Subdomains* that we need to use to delegate transversal processings. While strongly isolated from each other, they need to be integrated as a whole.

There are several strategies to address the underlying concerns of such integration, and this is purpose of the *Context Mapping* topic. This is a large and technical topic that will be covered in a later article.



## Aggregates, Entities and Value Objects

The concepts that live inside a Bounded Context are likely the *Aggregates* in your model. Here how they are defined by [Vaughn Vernon][]:

> Each *Aggregate* is composed of one or more *Entities*, where one *Entity* is called the *Aggregate Root*. *Aggregates* may also have *Value Objects* composed on them.

Well... I must admit that *Aggregates* are tough to get into and this definition does not help us very much. If like me you're coming from an *[Object-Oriented](https://en.wikipedia.org/wiki/Object#Computing)* mindset, understanding the nature of an *Aggregate* is not that easy. What is it? A *Class*? An arbitrary collection of *Objects*?

Before trying to define it more clearly, let's first look at the related concepts.

### Entity

> An *Entity* models an individual thing. Each *Entity* has a unique identity in that you can distinguish its individuality from among all other *Entities* of the same or a different type.
>
> Most times, an *Entity* will be mutable. The main thing that separates an *Entity* from other modeling tools is its uniqueness — its individuality.

Okay, this looks like more or less the concept of a *Class*, with a unique identifier property as we often use for describing *Domain Objects*.

### Root Entity

> The *Root Entity* of each *Aggregate* owns all the other elements clustered inside it. The name of the *Root Entity* is the *Aggregate*’s conceptual name. Choose a name that properly describes the conceptual whole that the *Aggregate* models.

What is meant by the *owns* relationship here? After some research, it means that the *Root Entity* itself is the actual representation of an *Aggregate*. This really is the entry point to any element which is part of the *Aggregate*. Any data can **only** be accessed through this *Root Entity*, and any operation on data is provided **only** by it.

Stated differently, the *Root Entity* is an *Entity* also assuming the role of *[Facade](https://en.wikipedia.org/wiki/Facade_pattern)* for the whole *Aggregate*.

Nice, we start to get a finer understanding of what an *Aggregate* is.



### Value Object

> A *Value Object*, or simply a *Value*, models an immutable conceptual whole. Within the model the *Value* is just that, a value. Unlike an *Entity*, it does not have a unique identity, and equivalence is determined by comparing the attributes encapsulated by the *Value* type. 

So this concept is similar to an *Immutable Class* for which equality is determined by comparing all its fields. A kind of "data holder" with a specific structure, but without any identity field.

### Consolidating the *Aggregate* concept

If I try to summarize a simple definition, we could say that:
> An *Aggregate* is a **conceptual** grouping of interrelated *Entities* and *Value Objects*. One of the *Entities* is the *Root Entity*, which is the **single entry point** to read and mutate data of any element of the *Aggregate*.

To make stronger decisions about what belongs or not to a given *Aggregate*, we must ensure that each *Aggregate* forms a *transactional consistency boundary*:
> Within a single *Aggregate*, all composed parts **must be consistent, according to business rules**, when the controlling transaction is committed. The reasons for the transactional boundary are **business motivated**, because it is the business that **determines what a valid state of the cluster should be** at any given time.

This principle seems to be the underlying reason for the *Root Entity* concept: as the single entry point to the *Aggregate*, it is responsible of maintaining data integrity among elements of the *Aggregate*. This has the benefit to avoid the *[Anemic Domain Model](http://www.martinfowler.com/bliki/AnemicDomainModel.html)* (anti-)pattern.

## Next step

In the [next article](/2017/02/26/Designing-Aggregates/), we will discuss the recommended approach to design good *Aggregates*.