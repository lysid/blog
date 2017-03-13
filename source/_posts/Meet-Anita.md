---
title: Meet Anita
date: 2016-11-15
categories:
  - Learning DDA
tags:
  - DDA
  - Scales
  - Persona
---

## A day in her life

Anita, 35, spends most of her time at patients' bedside, providing care and treatments to them. Taking care of other people is what drove her to choose her job and with years of experience, she emphasizes more and more  the human relationships with her patients.

<!-- more -->

When she has to evaluate the patient's status, she often uses one or more scales. She fills a paper form with some guidance. Back to the team room, she has to fill it back into a computer. Then she can make deeper analysis of various patients' indicators.

Like many of her colleagues, she feels like IT is more like a burden than an help in her daily activities. It surely has many advantages, but it can be time-consuming and break the flow of interactions with patients. Since a few months, the IT department has given her a small tablet (an iPad mini) that fits very well in her pockets. She is a bit afraid that it becomes even more intrusive between the patient and her.



## How we can help Anita

Thanks to our new [Scales][] application, Anita will be able to enter data right on her tablet. The clean and seemless interface allows her to focus first on her patients and forget about the tablet. At times, she may analyze the evolution of some key indicators from various scales to provide insights to her patient.

[Scales]: /products/scales



## Anita's goals and scenarios

*Note: we won't consider user and patient identification and authentication topics in the following scenarios. DDD speaking, we're designing for the [Scales][] bounded context only.*

### Goal #1: objectify her feeling about a patient's current status

Anita can quickly get some insights about her patient's clinical status by examining him. But sometimes she needs to make assumptions on less tangible elements. For example, evaluate patient's awareness after a head injury, based on how he reacts to specific stimuli. [Scales][Scales domain] come to be very handy in such cases, and they have formerly been proven to be reliable by a formal validation process.

As reliable as it can be, a Scale may be irrelevant if not used in the right context. **Anita must first choose the scale that will provide the most useful insights in a given situation.** This is the starting point of our application, that we can decline in two scenarios.

[Scales domain]: /2016/06/07/Introducing-the-Scales-domain/

#### Scenario #1.1: Anita is not confident about which scale to use

Anita is not confident about which scale (if any at all) she could use to get insights on a particular topic. She knows some *usual* scales almost by heart, but for this particular topic, she wonders if there could be a more specific scale to help her asses the situation.

<dl><dt>Story #1.1.1</dt><dd>Anita wants to enter some keywords in order to check if any scale could help her on a given topic.	</dd>
<dt>Story #1.1.2</dt><dd>Anita wants to look at a scale's detailed description in order to asses if it is relevant to her given situation.</dd>
<dt>Story #1.1.3</dt><dd>Anita wants to begin a new scale right from its detailed description in order to start using it immediately.</dd></dl>

#### Scenario #1.2: Anita knows exactly the scale she wants to use

In this particular case, Anita knows exactly which scale to use and she wants to begin a new evaluation as quick as possible.

<dl><dt>Story #1.2.1</dt><dd>Anita wants to pick a scale from the ones that have already been used for this patient (including by her colleagues) in order to assess the same scale at regular intervals in time.</dd>
<dt>Story #1.2.2</dt><dd>Anita wants to add a scale to its *Favorites* so that she can quickly retrieve it the next time.</dd>
<dt>Story #1.2.3</dt><dd>Anita wants to pick a scale from the ones she has added as a *Favorite* in order to start using it immediately.</dd>
<dt>Story #1.2.4</dt><dd>Anita wants to type some letters from the name of a scale she already knows in order to start using it without having to browse all the catalog</dd></dl>

#### Scenario #1.3: Anita gets insights from the results of a scale

Evaluating a scale means providing values for the *indicators* of the scale. There are several types of indicators: numerical values, list of items, graphical, etc. The result of the scale is most often a *Score* calculated from the values of the indicators.

*Note: there may be many types of indicators and behaviors among scales. Describing all of them here would be very fastidious, so we will stick to describing the general concepts.*

<dl><dt>Story #1.3.1</dt><dd>Anita wants to begin an evaluation of a given Scale in order to produce a *Score*.</dd>
<dt>Story #1.3.2</dt><dd>Anita wants to enter the value of an *Indicator* in order to report the corresponding patient's status into the system.</dd>
<dt>Story #1.3.3</dt><dd>Anita wants to go back and forth between the indicators of a Scale in order to review and correct previous answers.</dd>
<dt>Story #1.3.4</dt><dd>Anita wants to cancel the current evaluation so that she can start a new evaluation for maybe another scale.</dd>
<dt>Story #1.3.5</dt><dd>Anita wants to view the final *Score* of the scale and the corresponding *Interpretation* in order to get insights about her patient's status.</dd></dl>


### Goal #2: analyse trends on key indicators

Anita uses various Scales to evaluate her patient's current status at a given point in time. She is also interested in following the evolution of some specific *Indicators*. An *Indicator* is an individual element within a *Scale*: a question, a measure, a calculated score, etc.

#### Scenario #2.1: Anita has a quick look at existing evaluations

Before starting a new evaluation, Anita wants to check existing evaluations for her patient and get insights from them.

<dl><dt>Story #2.1.1</dt><dd>Anita wants to see an overview of all evaluations made for the patient in order to get an idea of his past or current status.</dd>
<dt>Story #2.1.2</dt><dd>Anita wants to see the details from an evaluation in order to get more insights from it.</dd>
<dt>Story #2.1.3</dt><dd>Anita wants to invalidate a previous evaluation, with a justification, in order to tell the system that this evaluation should no longer be considered nor displayed.</dd></dl>

#### Scenario #2.2: Anita builds a graph of indicator values

From the existing indicator values, Anita builds a graph of selected indicators. She may save it as a *View* for easier later reference.

<dl><dt>Story #2.2.1</dt><dd>Anita wants to pick an indicator for which values are available in order to follow its evolution on a suitable graphical representation.</dd>
<dt>Story #2.2.2</dt><dd>Anita wants to add other indicators to the graph in order to consider several aspects of the patient's status.</dd>
<dt>Story #2.2.3</dt><dd>Anita wants to save the current graph configuration as a named *View* in order to quickly retrieve this particular set of indicators later.</dd>
<dt>Story #2.2.4</dt><dd>Anita wants to share her *View* with her colleagues so that they can pick them for their own use.</dd></dl>



## Next step

In this article we met Anita and described her main scenarios and stories. Now we know her better, we can start brainstorming about her business workflow to identify key actions, events and concepts in the [next article](/2017/02/21/Event-Storming-Anitas-scenarios).