---
title: Event Storming Anita's scenarios
date: 2017-02-21
publish: true
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - Scales
  - "Bounded Context"
  - "Event Storming"
  - "Ubiquitous Language"
  - Aggregate
  - "Domain Event"
  - Command
---

Now we know a little about [Anita][] and the way she wants to interact with our [Scales][] application, we need to identify the core concepts from the [DDD][Domain-Driven Design] methodology. [Vaughn Vernon][], in his book *[Domain Driven Design Distilled][dddd-book]*, describes a participating approach he calls *Event Storming*. Check for my [personal summary][dddd-summary-pdf] for an overview of this approach.

[Anita]: /2016/11/15/Meet-Anita/
[Scales]: /products/scales
[Domain-Driven Design]: https://en.wikipedia.org/wiki/Domain-driven_design
[Vaughn Vernon]: http://vaughnvernon.co
[dddd-book]: http://amzn.com/0134434420
[dddd-summary-pdf]: /assets/DDD-Distilled-Summary.pdf

In a few words, using sticky notes over a very large wall where time flows horizontally, we follow these steps:
1. identify the *Domain Events* sequence
2. create *Commands* that trigger these events
3. identify first *Aggregates* contextual to these events
4. identify key UI views and internal processes
5. establish the *Ubiquitous Language* within our *Bounded Context*

<!-- more -->



## Anita's scenarios Event Storming

Following this approach, we created the following diagrams:

### Scenario #1.1
{% img /assets/ES_Anita_Scenario_1.1.gif "Anita - Event Storming Scenario #1.1" %}

### Scenario #1.2
{% img /assets/ES_Anita_Scenario_1.2.gif "Anita - Event Storming Scenario #1.2" %}

### Scenario #1.3
{% img /assets/ES_Anita_Scenario_1.3_1.gif "Anita - Event Storming Scenario #1.3 (part 1)" %}
{% img /assets/ES_Anita_Scenario_1.3_2.gif "Anita - Event Storming Scenario #1.3 (part 2)" %}

### Scenario #2.1
{% img /assets/ES_Anita_Scenario_2.1.gif "Anita - Event Storming Scenario #2.1" %}

### Scenario #2.2
{% img /assets/ES_Anita_Scenario_2.2.gif "Anita - Event Storming Scenario #2.2" %}



## Our Ubiquitous Language

Running the *Event Storming* session allowed us to produce the key elements of our Scales' *Ubiquitous Language*.

Here is a quick definition of a *Ubiquitous Language* (excerpt from [Domain Driven Design Distilled][dddd-book]): 
> Within a Bounded Context, the language is called the « Ubiquitous Language » because it is both spoken among the team members and implemented in the software model. When someone on the team uses expressions from the Ubiquitous Language, everyone on the team understands what is meant with precision and constraints.

In our [Scales][] bounded context, we identified the following concepts:
<dl><dt>Scale</dt><dd>A *Scale* refers to the description of a series of *Indicators* from which one or more *Scores* will be calculated.<br>Beware not to confuse a *Scale* with a *Scale evaluation*: the *Scale* is the description itself of a list of indicators while a *Scale evaluation* is the data entered and the resulting scores. Eventually, there will be several *Scale evaluations* for any single *Scale*.</dd>
<dt>Indicator</dt><dd>An *Indicator* is the description of a measure, its type, etc. that can be used to compute one or more *Scores*. There can be several types of *Indicators*: text, number, list, etc. A *Score* is a specific kind of *Indicator* that is computed from other *Indicators*.<br>Beware not to confuse an *Indicator* with an *Indicator evaluation*: the *Indicator* is the description itself of a measure while the *Indicator evaluation* is the measured value. Eventually, there will be several *Indicator evaluations* for any single *Indicator*.</dd>
<dt>Scale evaluation</dt><dd>The sum of *Indicator evaluations* (data entered as well as computed *Scores*) as per the *Scale* definition. See the *Scale* definition for disambiguation.</dd>
<dt>Indicator evaluation</dt><dd>The data entered by the user or computed from other indicators in the case of a *Score*. See the *Indicator* definition for disambiguation.</dd>
<dt>Indicator view</dt><dd>A graphical representation of one or more *Indicator evaluations* over time. This view could be shared among users in various ways.</dd></dl>


## Next step

Now we clarified many aspects of the business, we can start designing our software artifacts: *Aggregates*, *Commands* and *Domain Events*. This is the topic of a future article, but we need first to [dig into the core DDD concepts](/2017/02/22/Digging-into-core-DDD-concepts).

