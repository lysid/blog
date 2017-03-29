---
title: The Indicator Aggregate
date: 2017-03-13
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - Scales
  - Domain
  - Aggregate
  - Entity
  - "Value Object"
  - "Ubiquitous Language"
---

We are in the process of designing the initial form of the *Aggregates* of our [Scales][] domain. In this article, we will discuss how we design the *Indicator* *Aggregate*, starting from its *Entity* definition and enriching it using business rules we collected during previous discussions with the business.

[DDD]: https://en.wikipedia.org/wiki/Domain-driven_design
[Scales]: /products/scales

<!-- more -->



## Definition and first business rules

> An *Indicator* is the description of a measure, its type, etc. that can be used to compute one or more *Scores*. There can be several types of *Indicators*: text, number, list, etc. A *Score* is a specific kind of *Indicator* that is computed from other *Indicators*.
> Beware not to confuse an *Indicator* with an *Indicator evaluation*: the *Indicator* is the description itself of a measure while the *Indicator evaluation* is the measured value. Eventually, there will be several *Indicator evaluations* for any single *Indicator*.

Here are some additional business rules that may help us to make design decisions:
- There are two kinds of *Indicators*: those who can translate to a numeric value (althoug their visual appearance may not be numeric), named *Numerical Indicators*, and those who cannot (informative, contextual data), named *Informative Indicators*.
- *Numerical Indicators* may be entered in two ways: from a finite list of allowed values (named *FiniteListIndicators*) or from a continuous range of values (named *RangeIndicators*).
- *RangeIndicators* have two kinds of boundaries: the *nominal* boundaries (usually observed boundaries that may be overstepped on rare occasions) and the *strict* boundaries, which must be strictly observed.
- There may be a "conversion" logic between the value that is actually entered by the user (named *Indicator Value*) and the value used to compute the total score of the scale (named *Indicator Score*).

The first model of our *Indicator* aggregate looks like this:

{% plantuml %}
  class Indicator <<(R, lightgreen) Aggregate Root>> {
    version: Integer
  }
  enum IndicatorStatus << Enum >> {
    Draft
    Published
    Archived
    Evolved
  }
  class IndicatorId <<(V, lightblue) Value Object>> {
    uuid: String
  }
  class InformativeIndicator <<(R, lightgreen)>> {
  }
  class NumericalIndicator <<(R, lightgreen) abstract>> {
    conversionFormula: String
  }
  class FiniteListIndicator <<(R, lightgreen) abstract>> {
  }
  class ImageListIndicator <<(R, lightgreen)>> {
    allowedImages: Array;
  }
  class LabelListIndicator <<(R, lightgreen>>)>> {
    allowedLabels: Array;
  }
  class RangeIndicator <<(R, lightgreen)>> {
    strictMin: Number
    nominalMin: Number
    strictMax: Number
    nominalMax: Number
  }
  class Score <<(R, lightgreen)>> {
  }
  note bottom of Score
    Inherited "conversionFormula" 
    has the meaning of "computeFormula" 
    in the context of Score.
  end note

  IndicatorStatus <- Indicator
  Indicator -> IndicatorId
  Indicator <|-- InformativeIndicator
  Indicator <|-- NumericalIndicator

  NumericalIndicator <|-- RangeIndicator
  NumericalIndicator <|-- FiniteListIndicator
  FiniteListIndicator <|-- ImageListIndicator
  FiniteListIndicator <|-- LabelListIndicator

  NumericalIndicator <|-- Score

  IndicatorId <--o Score : uses score from
{% endplantuml %}

So what we get is basically a single *Entity* (derived in several variations in sub-classes) acting as the *Aggregate Root*. This matches up with the first rule.



## Adding behavior

Let's now add business behaviour to our model. We are describing an *Indicator*, a business object used for modelling an aspect of a *Scale*. As we designed the preceding diagram, we realized that an *Indicator* can go through several statuses through its life cycle. Theses statuses are critical to the business because once an *Indicator* has been *published*, we must not change it otherwise *Indicator Instances* will link to an invalid description of their related *Indicator*.

So we defined the following rules with the business:
- When first *created*, an *Indicator* is in the *Draft* state. It can be modified at will while in this state.
- In order to be able to create *Indicator Instances* from it, the *Indicator* must then be *published*. This operation cannot be undone and the *Indicator* cannot be modified any longer while in this status.
- From there, two options arise:
  1. The *Indicator* can be *archived*, in which case it can no longer be used to create *Indicator instances*. This can be done only if all the *Scales* that use it are also *archived* (or if no *Scale* at all use it). *Unarchiving* is then a possible operation, reverting its state back to *Published*.
  2. The *Indicator* is *evolved into* a new one (thus having a new *IndicatorId*), and they are tightly coupled together as one is the replacement for the other.
  This *evolve into* relationship has a strong meaning in that the semantics of the *Indicators* remain the same between them. It is a matter of refined details of the semantically identical concept.
  When in *Evolved* state, *Indicator Instances* can still be created from a *published* *Scale* that uses it. But new or *Draft* *Scales* must use the replacing *Indicator*.
  *Evolutions* can be chained, reflecting several evolutions of semantically similar *Indicators*. The latest *Indicator* (end of the chain) must always be used.
  *Evolved* state cannot be undone.

Let's summarize theses rules into a simple *State diagram*:

{% plantuml %}
  [*] -> Draft
  Draft --> Draft : edit()
  Draft --> Published : publish()
  Published -left-> Archived : archive()\n[Not used by any Scale\nor used by Scales also Archived]
  Archived -> Published : unarchive()
  Published --> Evolved : evolveInto(indicator:IndicatorId)
  Evolved: evolvedInto: IndicatorId
{% endplantuml %}



## Improved model

We can then integrate these behaviors into our *Indicator Aggregate*:

{% plantuml %}
  class Indicator <<(R, lightgreen) Aggregate Root>> {
    version: Integer
    evolvedInto: IndicatorId
    publish()
    archive()
    unarchive()
    evolveInto(newIndicator:IndicatorId)
  }
  enum IndicatorStatus << Enum >> {
    Draft
    Published
    Archived
    Evolved
  }
  class IndicatorId <<(V, lightblue) Value Object>> {
    uuid: String
  }

  class InformativeIndicator <<(R, lightgreen)>> {
  }

  class NumericalIndicator <<(R, lightgreen) abstract>> {
    conversionFormula: String
  }

  class FiniteListIndicator <<(R, lightgreen) abstract>> {
  }
  class ImageListIndicator <<(R, lightgreen)>> {
    allowedImages: Array;
  }
  class LabelListIndicator <<(R, lightgreen>>)>> {
    allowedLabels: Array;
  }

  class RangeIndicator <<(R, lightgreen)>> {
    strictMin: Number
    nominalMin: Number
    strictMax: Number
    nominalMax: Number
  }

  class Score <<(R, lightgreen)>> {
  }
  note bottom of Score
    Inherited "conversionFormula" 
    has the meaning of "computeFormula" 
    in the context of Score.
  end note

  IndicatorStatus <- Indicator
  Indicator -> IndicatorId
  Indicator <|-- InformativeIndicator
  Indicator <|-- NumericalIndicator

  NumericalIndicator <|-- RangeIndicator
  NumericalIndicator <|-- FiniteListIndicator
  FiniteListIndicator <|-- ImageListIndicator
  FiniteListIndicator <|-- LabelListIndicator

  NumericalIndicator <|-- Score

  IndicatorId <--o Score : uses score from
{% endplantuml %}



## Let's code!

Although we most probably missed a lot of things yet, we can try to write a first implementation of the `IndicatorAggregate` class to get a sense of what [DDD][] looks like in code.

{% codeblock IndicatorAggregate.java lang:java %}
class IndicatorAggregate {
  
  private IndicatorId id;
  private Integer version;
  private IndicatorStatus status;
  private IndicatorId evolvedInto;
  
  public IndicatorAggregate(IndicatorId id) {
    this.id = id;
    this.version = 0;
    this.status = IndicatorStatus.Draft;
    this.evolvedInto = null;
  }
  
  public void publish() {
    this.status = IndicatorStatus.Published;
    this.version++;
  }
  
  public void archive() {
    if (somePublishedScalesAreUsingThisIndicator()) {
      throw new IllegalStateException("An indicator can be archived only if no Scale is using it or if all Scales using it are also archived.");
    }
    this.status = IndicatorStatus.Archived;
    this.version++;
  }
  
  public void unarchive() {
    this.status = IndicatorStatus.Published;
    this.version++;
  }
  
  public void evolveInto(IndicatorId newIndicator) {
    if (newIndicator == null) {
      throw new IllegalArgumentException("newIndicator must not be null");
    }
    this.status = IndicatorStatus.Evolved;
    this.version++;
    this.evolvedInto = newIndicator;
  }
}
{% endcodeblock %}

Pretty straightforward. My first feeling is that this code is written in a clear business-oriented style, exposing only business behaviours into public methods. Invariants are protected directly from within the *Aggregate*, which feels good. This way of writing code feels very natural as soon as you have a clear understanding of the *Ubiquitous Language* with clarified business rules.

Note: I did not add getters intentionally as I personally prefer to add them only as the need actually arises. When it's `private`, it's much safer ;)



## What about consistency?

We can see that there is not any major consistency issue, we only reference other *Indicators* by their *IndicatorId*. Yet we need to check if *Scales* are using the *Indicator* before archiving it, in the `somePublishedScalesAreUsingThisIndicator()` method. We will address this point later.



## Next step

In the next article, we will continue this exercise with the *Scales* *Aggregate*.

