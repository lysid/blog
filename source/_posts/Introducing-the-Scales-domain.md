---
title: Introducing the Scales domain
date: 2016-06-07
categories:
  - Learning DDA
tags:
  - DDD
  - Scales
---

## What are Scales about?

Throughout this series, we will build the core of the [Scales][] product. We won't detail every single aspect of it, but all the work will be open-sourced in our [GitHub repository][]. 

This product is not a simplified example, it has to be a shippable, production-grade product. And it will be shipped as soon as it is ready, maybe even before the final release.

*Scales* is a concept frequently used in the healthcare domain. A simple definition could be:
> *Scales* (a.k.a. *Scores*) allow to quantify phenomenons that cannot be measured physically and make assumptions based on the results.

Basically it consists in answering a list of predefined questions, resulting in a calculated *score* from which we can infer some assumptions.

[Scales]:				/products/scales
[GitHub repository]:	https://github.com/lysid/scales

<!-- more -->



## A word about personas

Coming from a usability background, I like to champion the concept of *[Personas][]*. I won't expand on this concept since it is a whole topic in itself. Quickly stated:

> *[Personas][]* are stereotypical characters, precisely described like real people, crafted from user research data.

One of their main benefit is to help make strategic decisions based on their profiles ; in our case to define more precisely the functional scope of our product.

We cannot afford to make a deep and formal user research for this product, but we've created some personas based on domain experts interviews. We feel sufficiently confident on their experience to start with these personas and improve them as we build and ship our product.

[Personas]: http://www.amazon.fr/dp/0125662513

In the [next article](/2016/11/15/Meet-Anita/), we will look at our first persona, Anita, clarify her goals and describe her usual scenarios.