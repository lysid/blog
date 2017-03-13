---
title: Learning DDD fundamentals
date: 2016-05-27
categories:
  - Learning DDA
tags:
  - DDD
  - Scales
---

[Domain-Driven Design][] is known to be hard to learn and relevant only for large complex projects. I'm a beginner on this topic (as you probably are if you're reading this), but my intuition tells that it must not be the case, and [I am not the only one to think so][ddd-not-that-hard]. The core principles are not that hard to understand, and they seem to fit well also on small projects.

As stated in [the first article from this series][kickoff article], we are about to build a quite small product. We will walk through all the steps together, learning and building all the way along. This will give us a pretty good idea of what it takes to apply the [DDD][Domain-Driven Design] principles on a small project.
<!-- more -->

[kickoff article]: /2016/05/16/Domain-Driven-Application
[ddd-not-that-hard]: http://blog.sapiensworks.com/post/2016/05/12/ddd-is-expensive-myth
[Domain-Driven Design]: https://en.wikipedia.org/wiki/Domain-driven_design



## What is "Domain Driven Design"?

You most probably already have a good general idea of what [Domain-Driven Design][] is all about. I won't elaborate on this since there are plenty of resources on the web that will introduce it to you. Go Google for them.

In a few words, I would describe it like this:
> Domain-Driven Design is a set of patterns and practices for creating a product, focusing at all levels on the domain concepts and expertise.

This a wide definition as it covers many aspects (project organization, processes, business analysis, software architecture and design, etc.). This is why it is somewhat impressive when you want to learn more. We will try to discover, understand and experiment it through the creation of the [Scales][] application.

[scales]: /products/scales



## Where to start from?

Well... This is the big question. 

The first book from [Eric Evans][], titled "[Domain-Driven Design][ddd-book]", is a huge one (more than 500 pages). To be honest, I did not read it and I'm not very much inclined to do so. There is [a free summary, "Domain-Driven Design Quickly"][ddd-book-summary] online, which is "only" 100 pages long... That said, both of them were written about a decade ago, and when I give a quick look at [the free summary][ddd-book-summary], I feel like things has evolved now and proposed practices may not be the current state-of-the-art.

Other people have applied DDD for a while now, and may propose more up-to-date introductions. One of them is [Vaughn Vernon][], who published a reference book in 2013, "[Implementing Domain-Driven Design][iddd-book]". Again, this is a comprehensive book about DDD (more than 600 pages), updated with feedback from his experience and advice on how to practically implement DDD. I did not read it yet, but I think **this should be the reference book to keep at hand** whenever you need to get insights about a particular DDD topic.

But **the real good starting point is the recently released "[Domain-Driven Design Distilled][dddd-book]" book** from the same author. This is a very clear and short introduction (about 170 pages) to the main concepts, along with the best practices you should adopt right from the beginning. A sample Scrum management tool project is used all along, explaining step by step how DDD helps you understand the domain and design your software. It contains many references to its companion reference book, "[Implementing Domain-Driven Design][iddd-book]", as well as other valuable resources.

As I often do with the books I find most interesting, I've created a [summary][dddd-summary-pdf] to get a very fast reminder of the core principles I can quickly read several times now and later. *Although it is based on excerpts from the book, this really is my personal notebook and is not intended to disclose book contents. If this constitutes a problem for anyone, please contact me and I will immediately remove it from this blog.*

If you know other valuable resources to get started quick and easy, feel free to tell us in the [comments](#comments).

## Resources

Most resources listed in this series will be centralized in the [DDA resources](/DDA-resources) page, along with a cached PDF export (when possible). 

[Eric Evans]: https://www.linkedin.com/in/ericevansddd
[ddd-book]: http://amzn.com/0321125215
[ddd-book-summary]: http://www.infoq.com/minibooks/domain-driven-design-quickly
[Vaughn Vernon]: http://vaughnvernon.co
[iddd-book]: http://amzn.com/0321834577
[dddd-book]: http://amzn.com/0134434420
[dddd-summary-pdf]: /assets/DDD-Distilled-Summary.pdf
