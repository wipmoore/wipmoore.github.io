---
layout: post
title: "Haskell function design"
date: 2018-01-12 06:40:00 +0000
categories: programming haskell
---

In chapter 6 of 'Programming in Haskell' there is a five step process for function design:

1. Define the type
2. Enumerate the cases
3. Define the simple cases
4. Define the other cases
5. Simplify and generalise


#### Define the type

Thinking about types is very helpfull it provides an understanding of the shape of our function and provides the information needed to think about the next step.

#### Enumerate the cases

For most types of argument there are a number of standard cases to consider. e.g. List have the empty and non-empty case.  This step is about identifying for the combination of arguments what cases need to be considered.

#### Define the simple cases

From the cases identified in the previous step implemente the simple cases first.

#### Define the other cases

Implement the remaining cases.

#### Simplify and generalise

Once a function has been defined it often becomes clear that it can be generalised and simplified.

## References

- [Programming in Haskell](http://www.foyles.co.uk/witem/computing-it/programming-in-haskell,graham-hutton-9781316626221)

## Presentations

* [Why Haskell](/presentations/why-haskell)
