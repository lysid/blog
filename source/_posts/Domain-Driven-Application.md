---
title: The Domain Driven Applications series
date: 2016-05-16
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - CQRS
  - Event Sourcing
  - Scales
---


## What is a "Domain Driven Application"?

This is a term of my own, although its purpose is not to create yet-another-crappy-acronym.  I just needed a name to designate an application created with a global domain-driven approach, beyond the purely architectural aspects.



## What is the purpose of this series?

> Create the [Scales][] application from scratch, while learning and applying some of the [DDD principles][] alongside with the [CQRS principle][]/[Event Sourcing][] architectural concepts. We will also try to go beyond the usual server-side considerations and think about how we could apply these concepts to the client side of our application. All this work will be released as open-sourced software under the (yet-to-be-determined) licence in a [GitHub project repository][].
> 
> <strong>In the long run, the ultimate goal is to provide a pragmatic and pleasant way to learn these powerful concepts and show a way to apply them to your own projects.</strong>
<!-- more -->

The starting point of this series is the [Scales][] project. I was asked to create this product for a not-for-profit foundation which couldn't afford to have a dedicated IT team managing commercial software from big companies. As I am moving from my [Flex][] expertise towards the trendy HTML/JavaScript technologies, this is a perfect candidate to learn these and showcase my fresh new skills. This is also one of the reason this project is released as open-sourced software under the (yet-to-be-determined) licence.

As [I have worked in the healthcare domain][LinkedInProfile] for several years now, I came to realize that this is an always-moving domain from a pure business point of view. Although it may look like a very well-established science, new practices emerge often, as well as new regulations and financial directives. At the same time, softwares have a very long lifespan (often more than a dozen years), leading to unstable data structures and frequently changing business rules.

When starting to think about the [Scales][] project, I recalled [a very interesting QCon keynote][infoq video] from [(author)][infoq video author] about the benefits of [Event Sourcing][], based on his experience in the finance field. I started to harvest more info about [Event Sourcing][], then moving to the intimately related [CQRS principle][], itself being a good architectural approach to create software according to the [DDD principles][]...

More often than not, people tend to use these three paradigms altogether, because they seem to provide a cohesive whole centered on the business domain first. This is a huge amount of new concepts and skills to learn, and it is quite difficult to decide where to start from...

**This is the main purpose of this series: learn and apply at the same time. We will be asking questions and discussing possible answers according to the current literature, then go implement the actual product based on our decisions.**

[Scales]:						/products/scales
[GitHub project repository]:	https://github.com/lysid/scales
[flex]:							https://flex.apache.org
[LinkedInProfile]:				https://www.linkedin.com/in/fredericmonjo
[infoq video]:					https://www.infoq.com/presentations/Event-Sourced-Architectures-for-High-Availability
[infoq video author]:			https://www.linkedin.com/in/martinjthompson
[event sourcing]:				http://docs.geteventstore.com/introduction/event-sourcing-basics/
[CQRS principle]:				http://udidahan.com/2009/12/09/clarified-cqrs/
[DDD principles]:				https://en.wikipedia.org/wiki/Domain-driven_design



## What is the scope of this series?

> Any user story that brings interesting questions and discussions around the aforementioned concepts.

Although we could speak about application creation from the very beginning (I mean from the very first discussion with the customer), this would include fields like agile project management, user-centered designed, usability, etc. This could be a very interesting area to elaborate on, but as of now this is too much work for this humble blog.

To keep the scope of this series reasonable, we will begin our journey from a supposedly [Agile][] project which produced a [backlog][] of [User Stories][] (actually they are based on an existing product as well as envisioned improvements for the coming trends in healthcare).

Purely technical articles will be issued in their respective categories in this blog, although we may provide link to them when relevant.

**We won't detail every single user story but rather talk about those ones that bring questions and open discussions on key concepts and decisions to make.** 

[agile]:        https://en.wikipedia.org/wiki/Agile_software_development
[backlog]:      https://en.wikipedia.org/wiki/Scrum_(software_development)#Product_backlog
[user stories]: https://en.wikipedia.org/wiki/User_story



## Agenda

To build our application, we will try to be as much *Agile* as we can and produce potentially shippable products as often as we can. However the theoretical field is huge (take the reference [Domain Driven Design][] book by [Eric Evans][] as an example: more than 500 pages!) and we won't go very far without a minimal amount of theory. I will try to alternate both kinds of articles to make the journey as fun and practical as I can.

Obviously your [comments](#comments) are welcome all the way along, and I will happily update the article with better insights 

> **Disclaimer**: please keep in mind that **I am as much as a newbie as you may be**, so don't use the contents of this blog blindly. Always consider alternative opinions and approaches, and make your own judgement call on what you should or shouldn't do. **I can't be taken for responsible in any way for any damage that you could cause by following advice from this blog without further thinking required by your specific context.**

[Domain Driven Design]: http://amzn.com/0321125215
[Eric Evans]: https://www.linkedin.com/in/ericevansddd

All the articles in this series are listed below in a logical walk through order (which may not match their publication order):
1. [Learning DDD Fundamentals](/2016/05/27/Learning-DDD-fundamentals)
2. [Introducing the Scales domain](/2016/06/07/Introducing-the-Scales-domain)
3. [Meet Anita](/2016/11/15/Meet-Anita)
(to be continued)


## Resources

Most resources listed in this series will be centralized in the [DDA resources](/DDA-resources) page, along with a cached PDF export (when possible). Wikipedia articles will not be included as they are the easiest source of information an almost any topic.