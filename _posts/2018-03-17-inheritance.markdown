---
layout: post
title: "Inheritance"
date: 2018-03-17 20:00:00 +0100
categories: software-engineering
---

* Every use of inheritance should belong to one of the accepted categories.
* Every use of inheritance should preferably belong to just one of the accepted categories.

## Categories

* Subtype
* Restriction
* Extension
* Variation
* Uneffecting
* Reification
* Structure
* Implementation
* Facility

### Subtype

If B is a subset of A and any other subset of A is disjoint from B ( A ∩ B = ⦳ ) then A must be defered.

### Restriction

If B is a subset of A and instances of B are thoes that satisfy a certain constraint.  The constraint should be expressed as part of the invariant of B and not A.  Any feature added to B should be a logical consequence of the added constraint.  Both A and B should be either deferred or effective.

### Extension

If B is a subset of A and B introduces features not present in A and not applicable to direct instances of A. A must be effective.

### Variation

If B is a subset of A and B redefines some features of A, B must not introduce any features that are not direct needs of the redefined feature. There are two types:

* _Function_ where the redefinition affects feature body rather than just their signature.
* _Type_ where all redefinitions are signature redefinitions.

Both A and B should be either deffered or effective.

### Uneffecting

If B is a subset of A and B redefines some of the effective features of A into deffered features.  A will normally be effective and B will be deffered.

### Reification

If B is a subset of A where A represents some kind of data structure and B represents a partial or complete choice of implementation of A.  A will be deffered and B may be deffered or effective.

### Structure

If A represents a general structural property such as being comparable and B represents a certain type of object the posses that property.  A will be deffered and B may be deffered or effective.

### Implementation

If B obtains from A a set of features necessary to the implementation of the abstraction associated with B. Both A and B must be effective.

### Facility

Applies if A exists solely for the purpose of providing a set of logically related features for the benefit of it heirs.  Two variants are:

* _Constant_ where all features of A are constants.
* _Machine_ where the features of A are routines which may be viewed as operations on an abstract machine.

## References

* [Object-Oriented Software Construction](https://www.amazon.co.uk/Object-Oriented-Software-Construction-Prentice-Hall-Resource/dp/0136291554/ref=sr_1_3?ie=UTF8&qid=1521323632&sr=8-3&keywords=object+oriented+software+construction)