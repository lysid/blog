---
title: Designing Aggregates
date: 2017-02-26
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - Scales
  - "Bounded Context"
  - Domain
  - "Eventual Consistency"
  - "Event Sourcing"
  - Aggregate
  - Entity
  - "Value Object"
  - "Domain Event"
---

In the [previous article][] we discovered the core [DDD][Domain-Driven Design] concepts. We now understand better how our *Domain* is made up of *Bounded contexts* in which we use a specific *Ubiquitous Language* to describe business things. These business concepts from this language are modeled through *Aggregates*, *Entities* and *Value Objects*.

In this article, we will look at general guidelines to designing *Aggregates* and apply them to create our first *Aggregates* for the [Scales][] product.

[Domain-Driven Design]: https://en.wikipedia.org/wiki/Domain-driven_design
[previous article]: /2017/02/22/Digging-into-core-DDD-concepts/
[Scales]: /products/scales

<!-- more -->

Let's remind the short definition we used in the [previous article][]:
> An *Aggregate* is a **conceptual** grouping of interrelated *Entities* and *Value Objects*. One of the *Entities* is the *Root Entity*, which is the **single entry point** to read and mutate data of any element of the *Aggregate*.

This general definition does not provide any clue about how to design *Aggregates*. As usual, I will be quoting from the *[Domain Driven Design Distilled][dddd-book]* by [Vaughn Vernon][] to get some guidance on this topic.

[Vaughn Vernon]: http://vaughnvernon.co
[dddd-book]: http://amzn.com/0134434420
[dddd-summary-pdf]: /assets/DDD-Distilled-Summary.pdf



## General rules about *Aggregates*

Before talking about *how* to design *Aggregates*, let's first look at some general rules we should always keep in mind to help us choose among our design options.

### The *Aggregate* primarily protects the *Business invariants* inside its boundaries

We already discussed this rule in the [previous article](/2017/02/22/Digging-into-core-DDD-concepts/#Consolidating-the-Aggregate-concept). An *Aggregate* defines a *transactional boundary* for **business rules**. It may as well be implemented as a technical transaction in the persistence storage, but this is not what is driving the decision. Again:
> Within a single *Aggregate*, all composed parts **must be consistent, according to business rules**, when the controlling transaction is committed. The reasons for the transactional boundary are **business motivated**, because it is the business that **determines what a valid state of the cluster should be** at any given time.

This really is the most important rule to follow as it is business-driven.

### The *Root Entity* is the single entry point to the *Aggregate*

We already discussed this rule in the [previous article](/2017/02/22/Digging-into-core-DDD-concepts/#Root-Entity). Any element of the *Aggregate* is accessed only through this *Root Entity*, and it is the orchestrator of any mutation of data inside the *Aggregate*.

### Design small *Aggregates*

The two preceding rules naturally lead to this third one ; the smaller your aggregate the faster and reliable it will be. Having huge *Aggregates* leads to more complex business logic implementation and improved probability for failed transactions. Following the [*Single Responsibility Principle*](https://en.wikipedia.org/wiki/Single_responsibility_principle) helps to keep the *Aggregates* small.

This rule is also very important and we will see how to implement it using several techniques later on.

### Reference other *Aggregates* by **identity only**

This another natural consequence of the previous rules. Usually a domain model has many relationships between entities and we are used to model the whole thing as one big single model. But since we want to keep our *Aggregates* small, the [DDD][Domain-Driven Design] approach favors several *Aggregates* that reference each other **by identity** only. Which leads to the next rule.

*Note: a side benefit of this rule is that we can easily store our *Aggregates* in almost any kind of persistence mechanism (relational, document-based, key-value based, etc.).*

### Update other *Aggregates* using *Eventual Consistency*

*Eventual Consistency* is a cornerstone of the *Event Sourcing* approach. It just means that consistency over several *Aggregates* will occur with some delay, it will not be enforced as a single transaction for all *Aggregates*. This is kind of *asynchronous reactive design* based on *Domain Events*:
1. The first *Aggregate* mutates its data and commits its transaction. The output of this commit is a *Domain Event* describing what has changed from a business point of view.
2. This *Domain Event* is dispatched through a *Messaging mechanism* to whoever is interested in this particular event.
3. Interested *Aggregates* (which may include the originating *Aggregate* itself) reacts to this *Domain Event* by mutating their data in turn and output a new *Domain Event*. And so on.

Note: we will address this specific topic from a technical point of view in a later article.

### A note on Design traps we should avoid

As our design decisions are driven by the business, we must keep using the *Ubiquitous Language* and make sure it is perfectly clear to both business and technical people.

As developers we tend to let our technical concerns make their way into the business. For example, we like to abstract many things in the hope they will be more reusable. We should avoid that as much as we can and keep the *Ubiquitous Language* as close the business as we can. We can always evolve it later, especially if we follow an *Event Sourcing* approach.

Another point we should care about is where we implement the business invariants. As protecting them is the key objective of *Aggregates*, we must implement their validation logic **inside** the domain objects, so that we cannot put the *Aggregate* in an inconsistent state by side-effects or wrong usage. Put differently: avoid the *[Anemic Domain Model](http://www.martinfowler.com/bliki/AnemicDomainModel.html)* (anti-)pattern.



## A framework for designing our *Aggregates*

[Vaughn Vernon][], in his *[Domain Driven Design Distilled][dddd-book]* book, insists on the importance of designing *Aggregates* with the right size:
> Prevent the design of large clusters while still maintaining consistency boundaries that will protect true business invariants.

The following design steps will help us reach our consistency boundary goals:

>  1. Start by creating **every *Aggregate* with just one *Entity***, which will serve as the *Aggregate Root*. Don’t even dare to place two Entities in a single boundary.
  
>  2. Populate each of the *Entities* with the fields that you believe are most closely associated with the single *Root Entity*. One big hint here is to define every field that is required to identify and find the *Aggregate*, as well as any additional intrinsic fields that are required for the *Aggregate* to be constructed and left in a valid initial state.

>  3. Look at each of your *Aggregates* one at a time, and ask the domain experts if any other *Aggregates* you have defined must be updated in reaction to changes made to *Aggregate A1*. Now ask the domain experts how much time may elapse until each of the reaction-based updates may take place. This will lead to two kinds of specifications:  
>  &nbsp;&nbsp;&nbsp; a. immediately, or  
>  &nbsp;&nbsp;&nbsp; b. within N seconds/minutes/hours/days.  
>  One possible way to find the correct business threshold is by presenting an exaggerated time frame (such as weeks or months) that is obviously unacceptable. This will likely cause business experts to respond with an acceptable time frame.

>  4. For each of the immediate time frames (3a), you should strongly consider composing those two *Entities* within the same *Aggregate* boundary. That means, for example, that *Aggregate A1* and *Aggregate A2* will actually be composed into a new *Aggregate A[1,2]*. Now *AggregatesA1* and *A2* as they were previously defined will no longer exist. There is only *Aggregate A[1,2]*.
  
>  5. For each of the reacting *Aggregates* that can be updated following a given elapsed time (3b), you will update these using *Eventual Consistency*.

> **Be careful that the business doesn’t insist that every *Aggregate* fall within the (3a) specification (immediate consistency)**. It is very unlikely that the business really needs immediate consistency in every case. Considering what the business would have to do if it ran its operations only by means of paper systems can provide some worthwhile insights into how various domain-driven operations should work within a software model of the business operations.

A side benefit of follwing these guidelines is that our *Aggregates* will be easier to test automatically. This an important benefit since we want to make sure our business rules are strongly implemented into our model.



## Next step

In the [next article](/2017/03/08/The-Scales-Aggregates/), we will apply this approach to create the first *Aggregates* for the [Scales][] application, based on [Anita's Event Storming output](/2017/02/21/Event-Storming-Anitas-scenarios/).