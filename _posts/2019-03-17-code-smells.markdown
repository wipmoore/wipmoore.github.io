---
layout: post
title: "Code Smells"
date: 2019-03-18 11:00:00 +0100
categories:
---

- Code bloat
    - [Large class](#large-class)
    - [Long method](#long-method)
    - [Long parameter list](#long-parameter-list)
    - [Data clump](#data-clump)
    - [Primitive obsession](#primitive-obsession)

- Tool Abuse
    - [Alternative classes with different interfaces](#alternative-classes-with-different-interfaces)
    - [Refused Bequest](#refused-bequest)
    - [Switch statements](#switch-statements)
    - [Temporary field](#temporary-field)

- Change prevention
    - [Divergent Change](#divergent-change)
    - [Shotgun Surgery](#shotgun-surgery)

- Dispensables
    - [Data class](#data-class)
    - [Duplicate code](#duplicate-code)
    - [Lazy class](#lazy-class)
    - [Speculative Generality](#speculative-generality)

- Coupling
    - [Feature envy](#feature-envy)
    - [Inappropriate intimacy](#inappropriate-intimacy)
    - [Message Chains](#message-chains)
    - [Middle man](#middle-man)

## Code bloat

### Large class


Classes with a large number of instance variables is generally an indication that it has more than one responsibility.  The maintenance issue is in the risk that you couple the two responsibilities together and then when you need to change one of the responsibilities you inadvertently change the coupled responsibility or at the very least it involves a more effort than it should as you need to check carefully that you have not changed the behaviour of the coupled responsibility.  This is fixed by identifying the different responsibilities and separating them out into their own class. 


### Long method

Long methods often contain repeated code that is implementing a specific behaviour, this is a maintenance issues as you if you need to change the behaviour you have to make a change in multiple places increasing the risk that you will either miss one or introduce different behaviour in different instances of the code.  This is fixed by breaking the long method into a series of smaller ones, this will force you to identify and remove duplication.  This should also result in code where the intent is easier to read if good names have been selected as you are not having to construct the intention from how you implemented it. 


### Long parameter list


This makes a method difficult to call and understand. It can be an indication that a method is performing multiple responsibilities which do not have to be performed at the same time i.e. it has low cohesion which is promoting bad coupling or that the parameters should be grouped into an object/structure.  If the method is performing multiple responsibilities then create a method for each responsibility, change the calling methods and then delete the original.  If there are multiple parameters from the same object/structure consider passing the object/structure in instead otherwise consider grouping the parameters into a object/structure that gives semantic meaning for the method.

### Data clump

This is when you have groups of data which is required for a specific part of the behaviour to be performed.  It is a maintenance issues in that it makes the code harder to understand as you are not expressing the fact that the set of values are required to perform a specific part of the behaviour.  This can be solved by grouping the data into an object/structure.

### Primitive obsession

This is where you are using primitive data types to represent item that should be models as a domain specific type. It leads to leaky abstraction that mean any operation on them have to perform excessive sanity checking before they can proceed this leads to an excessive amount of edge case checking for calling functions all of which make understanding the code harder. This can be fixed by representing the items as objects or immutable structures that have consistency constraints enforced in their construction and it this is an object are maintained when performing any of its operations.


## Tool Abuse

### Alternative classes with different interfaces

This is when two classes have different interfaces and yet the classes are quite similar. This implies that they represent the same concept meaning you have to implement the same behaviour ( be it in methods on the objects or functions ) for each similar class. This can be fixed by identifying the similarities between the two classes and either merging them into a single class or creating a common interface that both classes can support.


### Refused Bequest

This is where a subclass does not fully support its supper classes interface.  This breaks the liskov substitution principle resulting in abstractions needing to know about all implementations of the supper class or at least the ones that do not fully support it supper classes implementation.  If the subclass is not really an 'is a' member of its superclass but just needs to access some functionality from it parent you can remove the subclass from the inheritance hierarchy and add the superclass as a parameter to the appropriate methods.  If the inheritance is appropriate move all the unneeded field and methods from the superclass into an new sub class.


### Switch statements

This is when the same switch or if..else if..else statement is duplicated across the system.  It is a maintenance burden in that you have behaviour that represents a particular state spread across the multiple places in your code base so makes it hard to understand the behaviour that state represents.  Instead of splitting the behaviour for each specific case across multiple locations across the system it should be consolidates into a class or object instance for each specific case.


### Temporary field

This is when an object has a field that is only partially needed the rest of the time the field is empty of or contains irrelevant data that is difficult to understand.  This can be fixed by moving the field and all operations that use the field into a new class.


## Change prevention

### Divergent Change

This is where a class is commonly changed in different ways for different reasons.  The maintenance issue is in the risk that you couple the two responsibilities together and then when you need to change one of the responsibilities you inadvertently change the coupled responsibility or at the very least it involves a more effort than it should as you need to check carefully that you have not changed the behaviour of the coupled responsibility.  This is fixed by identifying the different responsibilities and separating them out into their own class. 


### Shotgun Surgery


This is when you must change lots of pieces of code in different places simply to add a new or extended piece of behavior, this can be an indication that a single responsibility has been split over multiple classes. This is a maintenance issue as it makes it hard to understand the behaviour associated with a specific responsibility and it takes a lot of energy to make simple changes.   This can be fixed by consolidating all the logic for the responsibility into a single class.


## Dispensables

### Data class

Classes that have fields and nothing else this breaks the encapsulation where an object has a public interface consisting of methods and the data it works being hidden in private fields.  I do not see this as a code smell so long as the class is immutable.


### Duplicate code

Generally this is where you have the same behaviour implemented multiple times usually on different data.  This is a maintenance burden as if you want to change some behaviour you have to modify it in multiple places, it also increases the risk that if you need to make a change to the behaviour you miss one of the implementation resulting in the system being left in an inconsistent state.  This is solved by moving the duplicate logic into a method. 


### Lazy class

This is when you have classes that do not do enough to justify their maintenance cost.  They are a maintenance burden as you have to understand them which and they add clutter to the project.  This can be fixed by moving the behaviour into the classes that use it.


### Speculative Generality

This is code that was added based on speculation that the functionality may be needed, there are two main sort domain logic that has not been asked for and or excessive factoring such as adding generic or helper classes that implement logic that is only used once.   This tends to make the code harder to understand and add extra code that has to be maintained even when it is not used.  The unneeded functionality should be removed from the codes base.


## Coupling

### Feature envy

When a method makes to many calls to other objects to obtain data or functionality.  This is bad because in oo data and the behaviour that act on it belong together so this implies that two objects are strongly coupled and not cohesive.  As there is a strong evidence that the two classes support the same responsibility the logic for that responsibility should be consolidated into a single class.


### Inappropriate intimacy 

Using private methods of another class. Internal implementation details of a class are exactly that internal and should not be known by any other class, this is essentially information hiding ( A fundamental of object orientation ) which limits the impact of changing how an object achieves its goal.  External classes should only use the public interface of a class. 


### Message Chains

When you see a long sequence of message calls, this is breaking the law of Demeter ( The principle of least knowledge ).  The law is intended to reduce coupling.


### Middle man

When all an object does is pass messages on to another object it provides very little value. This is a specialization of the Lazy class.  Note in messaging systems these can be good things as they are being explicit about where a message is sent.


## References

- [Smells to refactoring](https://www.industriallogic.com/wp-content/uploads/2005/09/smellstorefactorings.pdf)
- [refactoring guru](https://refactoring.guru/)

