---
layout: post
title:  "Simple vs Easy by Rich Hickey"
date:   2017-09-17 13:00:00 +0100
categories: designConcepts
---

Notes on Rich Hickeys [Simple Made Easy](https://www.youtube.com/watch?v=cSwPOpOKr3w) presentation at [The Strange Loop 2011](https://www.thestrangeloop.com/).

* [Summary](#summary)
* [Simple](#simple)
* [Easy](#easy)
* [Conclusion](#conclusion)

### Summary

Ease and simplicity are often misunderstood and assumed to be the same which is incorrect. They are both necessary but different properties of efficiency.

Simplicity is a measure of our ability to understand a thing and impacts our ability to change a system and how much waste will result when making a change.  Ease is a measure of how much energy is required to achieve a task and impacts on how productive we are.

Our capability to understand is finite and quite limited, so it is important that the systems we make do not exceed out ability to understand them.  Ease is subjective we can generally reduce the difficulty we have in achieving something, so the emphasise is on us to reduce the difficulty that we have in achieving something rather than being a property of the systems that we build.

When building a system simplicity and ease are important and neither should be ignored but the impact of ignoring simplicity is greater than the impact of ignoring ease.


### Simple

For the purpose of the presentation *simple* is compared against *complex*. The property being measured is our ability to be able to change a system over time. Where change can be any of: correction ( fixing errors ), adaption ( modifying the way an existing feature is performed ), or expansion ( adding new features ).  

The meaning of simple is taken as one fold by tracing its origin back to the [Proto-Indo-European(PIE)](https://en.wikipedia.org/wiki/Proto-Indo-Europeans) compound of sem- (one) and plo- (fold) and the meaning of complex is taken as many folds.  Complexity is seen in the sense of how you create a complex pattern or structure by braiding (plaiting) or weaving.

Given that the definition of simple is that it has one fold the ability to be able to change a system over time is seen as an *objective* property.

The strands that are weaved or plaited represent:

* Roles
* Tasks
* Concepts
* Dimensions

We all have roughly the same capabilities to comprehend complexity even the cleverest people do not have that much advantage over the average. Statistically the variation in peoples capacity to comprehend complexity is insignificant relative to the size of complexity we have the ability to create.

If you are going to change software you have to analyse what it does and make decisions about what it ought to do. What are the impacts of this change and what are the parts of the software that I have to go to to affect the change. If you can not reason about your program you can not make these decisions. We can not make something reliable that we do not understand.  There is a tradeoff between flexibility and understandability, flexibility generally leads to higher complexity.

If you ignore complexity you will slow down over time.  The implication being that complexity is our natural state so if you do not consciously pursue simplicity you will tend toward complexity and that complexity will kill you, every sprint will be about redoing things that you have already done. The net effect is you do not make progress.

Complexity limits our ability to understand a system and our capability to comprehend complexity is quite limited.  The only way we have to deal with this is to ensure that the systems we build are as simple as possible.  Some complexity will be inevitable as it will be part of the problem the system is solving this is know as essential complexity, complexity that is introduced via the tools we use and constructs we create is know as accidental complexity and should minimised.

> 'Simplicity is a prerequisite for reliability'  
> **Edsger W. Dijkstra**

Once a need to change a system has been identified analysis is required to understand what parts of the system have to be changed and how to apply the changes.  Analysis is only effective if the system is simple.

Testing by itself is not enough you can not just bypass analysis because you have tests. Every bug got passed the type checker and all the tests.  Tests can not guarantee the absences of errors, they are very unlikely to be exhaustive and suffers from the problem of induction.  Their purpose is not to decide what logic should be applied they are there to provide a level of confidence that the the logic you have applied is working as you expect it. Would having test suites and refactoring tools make changing a knitted castle
faster than changing a lego castle.

Analysis is not counter to agile principles as:

> 'Continuous attention to technical excellence and good design enhances agility.'

is an agile principle and part of the presentations argument is that simplicity is a necessary property of technical excellence and you can only achieve simplicity with analysis.

The benefits of simplicity are that it:

* Enables understanding.
* Facilitates change.
* Increased flexibility ( change policies move thing around because they are not interleaved.

> 'Truth is ever to be found in the simplicity, and not in the multiplicity and confusion of things.'<br>
> **Isaac Newton**

#### Techniques and constructs that introduce complexity

> 'The more elaborate his labyrinths, the further from the Sun his face.'<br>
> **Mikhail Naimy**

Braiding where a pattern or structure is formed by interlacing multiple strands together results in complexity.  When strands are intertwined together we lose the ability of comprehension because we have to understand all the intertwined things. The intertwining of strands that braiding produces is an inappropriate construction technique because it quickly results in constructs that exceed our ability to comprehend what they are doing.  

Complex constructs:

* **State** conflates value and time.
* **Objects** conflate state's identity and value.
* **Methods** conflate function and state, possibly namespaces.
* **Syntax** conflate meaning and order.
* **Inheritance** conflate types.
* **Matching** conflate multiple pairs of who and what.
* **Variables** conflate value and time.
* **Loops** conflate what you do and how you do it.
* **Fold** conflate what you do and the order in which it is done.
* **Actors** conflate what is going to be done and who is going to do it.
* **ORM** Oh my god.
* **Conditionals** conflate rules and program organisation.
* **Inconsistent** *means to stand apart*, this is inherently complex because you have to have a set of things that are apart and think about them together. Eventually consistency is challenging.

#### How do we make things simple.

>  'Simplicity is a great virtue but it requires hard work to achieve it and education to appreciate it. And to make matters worse: complexity sells better.' <br>
>  **Edsger W. Dijkstra**

In the presentation it is mentioned that functional composition as opposed to composition by braiding is a good technique to use to combine simple structures together but no elaboration is provided as to why this is better.

**Chose simple constructs.**

The same programs can be made with simpler constructs.  They allow you to focus on what the software is supposed to do rather than how you do it.

Simple constructs:

  * Values
  * Persistent collections
  * Data:
    * Maps
    * Sets
    * Linear
    * Trees **(?)**
    * Graphs **(?)**
  * Functions (Purity)
  * Namespaces
  * Polymorphism:
    * Clojure Protocols **(?)**
    * Haskel type classes **(?)**
  * Declarative data manipulation:
    * SQL
    * Linq
    * [DataLog](https://en.wikipedia.org/wiki/Datalog)
  * Consistency ( Transactions and Values )

Simple vs Complex constructs:

* Values / State, objects
* Functions and namespaces / methods
* Polymorphism / Inheritance, switch statements, pattern matching
* Data / Syntax
* Set functions / Imperative loops, fold
* Queues / Actors
* Declarative data manipulation (SQL) / ORM
* Rules / Conditional
* Consistency / Inconsistent

**Abstraction: ( write simple stuff )**

Abstraction means to draw things away, to draw away from the physical nature of something.  It is not hiding stuff.

Achieved by:

* Who/What aspects

    * What - do we want to acomplish
        * Sets of functions and give them namespaces
        * Interfaces
        * They should be small
        * Only use values and other interfaces.
        * Do not confuse with How.
            * Concrete class or function.
            * Beware of semantics that dictate how something should be done.
        * Strictly separate what from how

    * Who - is about data/entities
      * Build components from subcomponents.
      * You should have many subcomponents.

    * How - implementation code
      * Connect thing together using Polymorphism
      * Open polymorphism policy

    * When/where
      * Complicate by directly connected Objects
      * Stick a queue between objects

    * Why
      * Policy and rules
      * Rules system

    * Information is simple do not complicate it with objects use maps and sets
      directly.

### Easy

For the purpose of the presentation *easy* is compared against *hard*.  The property being measured is the amount of effort or time that is needs to complete a task.

Easy and hard are seen as a *subjective* measure in that what is easy for one person may be hard for another and vice versa.

The benefits of ease are that it:

* Enables productivity the less energy/time spent on one task means there is
  more energy/time to spend on others.

**NOTE** - Productivity is **not** the same as efficiency, to be efficient you have to be productive but you also need to minimise waste.  You may be very productive but if a large percentage of the output or expenditure of energy is waste you are not efficient.

#### What makes things hard

  * Things not being near at hand.
  * Lack of familiarity.
  * It is beyond our capabilities.

#### How do we make things easy.

**Make things close to hand**

Make sure that you have the tools that you need installed on your development environment.

**Learn**

Lack of familiarity can always be combated by learning. We always have capacity to learn new things.  

**Bring the problem within our capabilities**

Unlike familiarity which is seen as trivial to solve as we do not have limits on our ability to learn our capabilities are seen as being finite.  As such the way to deal with this is to bring the problem within our capabilities, what is meant by capabilities is our ability to understand complexity and the whole emphasis of the talk is that the way to deal with this is to build simple systems.

## Conclusion

Simple is the opposite of complex, it is concerned with measuring our ability to change a system over time.  Humans have a limited ability to understand complexity and unfortunately we tend to complexity (simplicity is hard) so if we do not actively work at simplicity we will build complexity in to the systems we create. This complexity kills our ability to change a system as we spend all our time redoing what has already been done.

There are two type of complexity essential which is the complexity built into the problem we are trying to solve and accidental which comes with the tools and constructs that we use to build systems.  The goal is to minimise accidental complexity.

Analysis is the fundament to building reliably, we will not be able to build things that we have any level of confidence in that the do what they are supposed to do. With out simplicity we can not effective analyse because our ability to analyse a thing is linked to its complexity.

Techniques such as static typing and automated test suites do not shield us from the need for analysis. Analysis is not counter to agility and the agile principles, it is in fact complementary to them as they stress the importance of technical excellence and simplicity which is achieved via analysis is an attribute of technical excellence.

A major way in which complexity is introduced into a system is in the constructs we use and techniques applied to combine constructs into a system.  Namely by using constructs that conflate multiple concerns (strands) and techniques that combine constructs in such a way that they are weaved (braiding) so tightly together that they form a complex pattern or structure which has to be understood in its entirety.  This can be avoided by using simple constructs, the correct application of abstraction when designing components and by constructing systems using functional composition.

In essence simplicity is about reducing then amount of waste in development and increasing the longevity (service life) of the system we build.

Easy is the opposite of hard, it is concerned with qualifying the amount of effort needed to complete a task. Ease is a subjective quality so we can make hard things easier.

Generally things are hard when we have not got the appropriate tools, we are working with things that are unfamiliar to us or are beyond our capability to understand them. These can be addressed by setting our environments up appropriately and increasing our familiarity through study and training.  We can not expand out capabilities so to address this we need to bring systems with in the range of our capabilities which is the role simplicity plays.

Ease is about being productive the easier thing are the more tasks we are able to complete.

Efficiency is high productivity and low waste, focusing on easy provides productivity and focusing on simplicity provides the low waste.  So to be efficient we need to pay attention to both easy and simplicity with a higher emphasis on simplicity as it is harder to attain and we only have finite capacity to deal with the consequences of ignoring it.

## Appendices

* [My Presentation](/presentations/simple-made-easy)
