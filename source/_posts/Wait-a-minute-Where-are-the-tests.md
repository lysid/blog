---
title: Wait a minute! Where are the tests?
date: 2017-03-28
categories:
  - Learning DDA
tags:
  - DDA
  - DDD
  - BDD
  - Scales
  - Aggregate
  - "Ubiquitous Language"
---

There is a consensus about the fact of writing automated tests, ideally before the code itself. If we are to write strong business code, we cannot afford to skip them for our *Aggregates*.

In this article, we will speak about the [Behaviour-Driven Development][BDD] approach, applied to our *Indicator* *Aggregate*.

[BDD]: https://en.wikipedia.org/wiki/Behavior-driven_development

<!-- more -->

## What is BDD?

I won't expand very much on the details of [Behaviour-Driven Development][BDD] (a.k.a. BDD) because this is not the purpose of this blog. Anyway, [BDD][] is basically the same philosophy as [Test-Driven Development][TDD] (a.k.a. TDD): write the automated tests first, then write the minimal code that makes the tests pass. Then add test cases and improve the business code, and so on.

[TDD]: https://en.wikipedia.org/wiki/Test-driven_development

[BDD][] extends on this approach. Whereas [TDD][] provides the methodology, it does not provide very much details on how to write meaningful test cases. Usually we create so-called "unit" tests, which are very technical and targeting small units of code.


### Formal language

In [BDD][] the focus is placed on the business rather than the technical details. The cornerstone of [BDD][] is its "language", written in plain text using a domain-specific human language:

> **Scenario**: something should respect some condition
>
> **Given** some initial context (pre-condition)
> **When** something happens (the behavior to test)
> **Then** some assertion should be verified (post-condition)

A *Scenario* is made up of a list of pre-conditions (*Given*, *And*, etc.), a behavior to be tested (*When*) and a list of post-conditions (*Then*, *And*, etc.). These assertions are written in business language, making this approach a prefect fit for our *Ubiquitous Language* specification.

The most beautiful thing in my opinion is that we then make these specifications executable. We have to write *glue code* to transform every assertion in some executable code.

**With [BDD][], we create executable specifications written in a human, domain-specific language. This is a perfect fit for crafting our *Ubiquitous Language* into executable source code.**


### Tooling

For [TDD][], the most used library is [JUnit][] (for Java, derivates exists for other languages), which provides a convenient API and is very well-integrated in almost all frameworks and IDEs. Most [BDD][] libraries can run on top of [JUnit][], taking advantage of its large ecosystem.

Although [BDD][] was initially implemented in Java into the [JBehave][] library, other implementations exist now. After spending several hours struggling with [JBehave][] (its API is quite unclear and its documentation is poor), I finally moved to [Cucumber][], which provides similar features. 

I took me a few minutes to get up and running with [Cucumber][], including integration into my [Maven][] build, which basically consist in: 
1. add cucumber dependencies to the Maven POM file
2. write a single empty class to ignite Cucumber on top of JUnit
3. write a *.feature* file in the *test resources* folders
4. write the corresponding glue code in a test class.

That's all we need to get started.

[JUnit]: http://junit.org/
[JBehave]: http://jbehave.org
[Cucumber]: https://cucumber.io
[Maven]: http://maven.apache.org


## The *Indicator* *Aggregate* specifications

In the [previous article](/2017/03/13/The-Indicator-Aggregate/), we already listed some business rules. I translated them into a [BDD][] specification. During the exercise, I refined the main rules into finer ones and discovered more test cases. 

Once the scenarios are written down in a plain text file ([Indicator.feature](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature) in this case), we can run them using a Cucumber launcher class ([QueryTest.java](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/java/studio/lysid/scales/query/QueryTest.java)), either from an IDE JUnit test launcher or from a [Maven][] build.

In the beginning, all the scenario steps will be skipped as they are considered *Pending*. We need to write the *glue code* implementing every step -- the actual testing code. This is done in a simple POJO class ([IndicatorSteps.java](https://github.com/lysid/scales/blob/bbc2bd7489df73cd8595c9aebde8ff03d26a582d/scales-query/src/test/java/studio/lysid/scales/query/indicator/IndicatorSteps.java)).

Now let's take some scenarios and see how we write the code to make them **actually executable**.


### An *Indicator* is created in *Draft* state

This is the simplest scenario we could write, checking that the initial state of an *Indicator* is *Draft*. The *When* assertion is the behavior under test (creating a new *Indicator*), the *Then* assertion tells what should be checked (its status is *Draft*). As you can see, it is written in human language using our *Ubiquitous Language* jargon:

{% codeblock Indicator.feature lang:gherkin https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature View on GitHub%}
  Scenario: An Indicator is created in Draft state
    When I create a new Indicator
    Then its status should be Draft
{% endcodeblock  %}

Each step in this scenario needs its own implementing method. In order to tell [Cucumber][] which method is implementing which step, we decorate them with specific annotations in which we provide the regular expression matching step's text:

{% codeblock IndicatorSteps.java lang:java https://github.com/lysid/scales/blob/bbc2bd7489df73cd8595c9aebde8ff03d26a582d/scales-query/src/test/java/studio/lysid/scales/query/indicator/IndicatorSteps.java View on GitHub%}
public class IndicatorSteps {
    
    private Exception thrownException;
    private IndicatorAggregate someIndicator;
    
    @When("^I create a new Indicator$")
    public void iCreateANewIndicator() {
        this.someIndicator = createIndicatorWithStatus(new IndicatorId("someIndicator"), null /* no status specified */);
    }
    
    @Then("^(?:its|the Indicator) status should be (Draft|Published|Archived|Evolved)$")
    public void itsStatusShouldBe(String statusName) {
        assertNull(this.thrownException);
        assertEquals(this.someIndicator.getStatus(), IndicatorStatus.valueOf(statusName));
    }
}
{% endcodeblock  %}

Using the power of regular expression we can match very specific text, like in our *Given* annotation, or match several variations of text that will all be implemented by this method. The *Then* annotation here is a good example:
- a first group `(?:its|the Indicator)` allows to match a step beginning by either *its status...* or *the Indicator status...* Which of the two is used has no interest for our test code (it is purely syntaxic), that's why we use a [non-capturing group](http://www.regular-expressions.info/brackets.html): it won't be injected into our parameters (see below).
- the second group defines a list of possible values and captures this value at the same time. This value is then injected into the `String statusName` parameter.

When combining all these features, we can write a single method with parameters that can be used for several variations of steps.

As a side note, you can see that writing tests in this manner is quite different from writing [JUnit][] tests, yet we still use its `assert*` statements. Since our test code is spread over several methods, we have to use class fields to store intermediate results. It's a bit confusing in the beginning, but we quickly get used to it.


### Publishing an *Indicator* changes its state into *Published*

This is also a straightforward example showing how we describe the initial context using the *Given* assertion:

{% codeblock Indicator.feature lang:gherkin https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature View on GitHub%}
  Scenario: Publishing an Indicator changes its state into Published
    Given a Draft Indicator
    When I publish this Indicator
    Then its status should be Published
{% endcodeblock  %}

We're building on top of the previous step implementations, so we don't have to write another method for the *Then* assertion, it's the same than in the previous scenario, but with another parameter value:

{% codeblock IndicatorSteps.java lang:java https://github.com/lysid/scales/blob/bbc2bd7489df73cd8595c9aebde8ff03d26a582d/scales-query/src/test/java/studio/lysid/scales/query/indicator/IndicatorSteps.java View on GitHub%}
public class IndicatorSteps {
    
    private Exception thrownException;
    private IndicatorAggregate someIndicator;
    
    @Given("^(?:a|an) (Draft|Published|Archived|Evolved) Indicator$")
    public void aStatusIndicator(String statusName) {
        this.someIndicator = createIndicatorWithStatus(new IndicatorId("someIndicator"), IndicatorStatus.valueOf(statusName));
    }

    @When("^I (publish|archive|unarchive) this Indicator$")
    public void iOperateThisIndicator(String operation) {
        this.thrownException = null;
        switch (operation) {
            case "publish":
                try {
                    this.someIndicator.publish();
                } catch (Exception e) {
                    this.thrownException = e;
                }
                break;
            (...)
        }
    }
    
    @Then("^(?:its|the Indicator) status should be (Draft|Published|Archived|Evolved)$")
    public void itsStatusShouldBe(String statusName) {
        assertNull(this.thrownException);
        assertEquals(this.someIndicator.getStatus(), IndicatorStatus.valueOf(statusName));
    }
}
{% endcodeblock  %}

The `@Given` annotation works exactly the same way than the previous ones. In this example, we've made it reusable by using a non-capturing group for syntaxic flexibility and another capturing group for status initialization.

The generic `iOperateThisIndicator()` method implementation may be overkill as it involves a `switch` statement with not much reused code. Anyway, there are two important things to note here:
- we capture the potential exception in a class field so that we can check it later in a *Then* assertion
- we must ensure that we correctly reset the captured exception to null at each call, otherwise it may stay in the way of future invocations.


### An *Indicator* can only be published when in Draft state

Previously we tested that the nominal behaviors work as expected. We also have to check that invalid behaviors fail adequately, in this case that an *Indicator* should not be *published* if it's in another state than *Draft* (e.g. *Published*, *Archived* or *Evolved*).

We could have to create one scenario for every possible initial state, but hopefully the [Cucumber][] syntax (called *Gherkin* syntax) supports *Scenario Outlines*. They work like a template in which some placeholders will be replaced by a list of values, virtually generating a series of scenarios:

{% codeblock Indicator.feature lang:gherkin https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature View on GitHub%}
  Scenario Outline: An Indicator can only be published when in Draft state
    Given a <initialState> Indicator
    When I publish this Indicator
    Then it should fail with message "An indicator can be published only when it has a Draft status."
    
    Examples:
      | initialState |
      | Published    |
      | Archived     |
      | Evolved      |
{% endcodeblock  %}

In this example, the scenario will be evaluated 3 times, one for each possible value of the *initialState* placeholder defined in the *Examples* section. This is a very convenient syntax to avoid duplicating lots of similar scenarios.

The *When* step is the same, so we have nothing to do for it. Since the `@Given iOperateThisIndicator()` method was implemented with a parameter for the possible states, it will match our *Given* step as well.

As for the *Then* step, we now make use of the stored Exception that should have been thrown by our *Indicator* *Aggregate* when invoking the *publish* behavior in an inappropriate state:


{% codeblock IndicatorSteps.java lang:java https://github.com/lysid/scales/blob/bbc2bd7489df73cd8595c9aebde8ff03d26a582d/scales-query/src/test/java/studio/lysid/scales/query/indicator/IndicatorSteps.java View on GitHub%}
public class IndicatorSteps {
    
    @Then("^it should fail with message \"([^\"]*)\"$")
    public void itShouldThrowAnIllegalStateExceptionWithMessage(String message) {
        assertNotNull(this.thrownException);
        assertEquals(this.thrownException.getClass(), IllegalStateException.class);
        assertEquals(this.thrownException.getMessage(), message);
    }
{% endcodeblock  %}

The capturing group for the error message is a bit tricky to get right. Hopefully, the IntelliJ Cucumber plugin automatically generates it when it detects double-quotes in the step text.

Although the type of Exception is not specified in the step definition (it has no meaning from a business point of view), we actually check it in the glue code, as well as its message contents.


## An *Indicator* cannot be archived if one of several *Scales* using it is not archived

This is an even more sophisticated scenario where several *Scales* are involved with various states. Using a scenario outline with two placeholders, we can generate a large number of scenarios with no effort:

{% codeblock Indicator.feature lang:gherkin https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature View on GitHub%}
  Scenario Outline: An Indicator cannot be archived if one of several scales using it is not archived
    Given a Published Indicator
    And a <firstScaleStatus> Scale using this Indicator
    And another <secondScaleStatus> Scale using this Indicator
    When I archive this Indicator
    Then it should fail with message "An indicator can be archived only if no Scale is using it or if all Scales using it are also archived."
    
    Examples:
      | firstScaleStatus | secondScaleStatus |
      | Draft            | Draft             |
      | Draft            | Published         |
      | Draft            | Archived          |
      | Draft            | Evolved           |
      | Published        | Draft             |
      | Published        | Published         |
      | Published        | Archived          |
      | Published        | Evolved           |
      | Archived         | Draft             |
      | Archived         | Published         |
      | Archived         | Evolved           |
      | Evolved          | Draft             |
      | Evolved          | Published         |
      | Evolved          | Archived          |
      | Evolved          | Evolved           |
{% endcodeblock  %}

I won't detail the glue code here, you got the point in the previous examples. An interesting thing however is when writing that code, I had to create [a minimalistic *ScaleAggregate* Java class](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/main/java/studio/lysid/scales/query/scale/ScaleAggregate.java) in order to make the tests pass. As with [TDD][], writing the tests first makes the code arise when necessary. That class will then be improved when we will create the *Scale* scenarios.

It also raised the tough question: *how to resolve dependencies between Aggregates?*. There are several possible answers that will be discussed in a later article. You can glance at the [*Indicator Aggregate's* source code](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/main/java/studio/lysid/scales/query/indicator/IndicatorAggregate.java) to get a clue about which approach feels best to me.


## Summary

That's well enough for this article! As we've seen, using the BDD approach has several advantages:
- the tests are written using our *Ubiquitous language* in a human-writable format
- these textual tests are **actually executed**, so they stand as a **guaranteed up-to-date human-readable specification** of our *Indicator* *Aggregate* (in addition to the [*Aggregate's* source code itself](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/main/java/studio/lysid/scales/query/indicator/IndicatorAggregate.java))
- writing tests this way promotes factorization of test code in reusable blocks
- thus adding test cases can be as easy and fast as copy/pasting textual scenarios with different values (or using *Scenario Outlines*).

You can [view the complete list of scenarios](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/resources/studio/lysid/scales/query/indicator/Indicator.feature) as well as [the complete glue code source](https://github.com/lysid/scales/blob/bbc2bd7489df73cd8595c9aebde8ff03d26a582d/scales-query/src/test/java/studio/lysid/scales/query/indicator/IndicatorSteps.java). You can even clone the repository and run the [QueryTest.java](https://github.com/lysid/scales/blob/7e884cb556f45a27d7ee8f578c32459685c3521a/scales-query/src/test/java/studio/lysid/scales/query/QueryTest.java) test launcher in your favorite IDE, or using `mvn test`. Don't try to run the actual project, it's an early proof-of-concept.

At the time of this writing, running the tests reports 34 scenarios executed (139 steps). I don't have the code coverage measure but it should be pretty good. In comparison to the implementation of the *Indicator* *Aggregate* from the previous article, its updated code has much improved. With a few dozens lines of scenarios and a similar amout of glue code, it's a satisfying performance in writing meaningful tests.

In the [next article](/2017/04/02/The-Scale-Aggregate/), we will design the *Scale* *Aggreggate*.