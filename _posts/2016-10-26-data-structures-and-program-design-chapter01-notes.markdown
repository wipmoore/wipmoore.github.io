---
layout: post
title:  "Chapter 1 - Data structures and program desing note"
date:   2016-10-22 15:48:00 +0100
categories: jekyll update
---

**Notes** Chapter 1 "Data Structures & Program Desing in C", ISBN: 0-13-519000-2.


## Programming Principles

**Include precise preconditions and postconditions with every function.**

* Always name your variables and functions with greate care and explain them thoroughly.
  * Names should express clearly the purpose of a class, function, variable.
  * Simple names for variables used briefly i.e. loop index.
  * avoid abbreviation.


**Keep documentation concise but descriptive**

* Place a prolog at the beginning of each function
  * Statement of purpose
  * Changes that the function makes to data
  * references to external data.
* For variables explain what its purpose is and how it is to be used.
* Do not parrot the code.
* Explain any statements that employ a trick.
* Documentation should explain why, code should explain how.


**The reading time for programs is much more than the writing time**

**Refinement & modularity**

* Do not lose sight of the forest for the trees.
* Divide problems into smaller problems that can be understood.


**Each function should do only one task, but do it well**

* If you are strugglig to concisely describe a function then it is probably doing too much.


**Each function sholuld hide something**

* Specify precisely the pre and post conditions, input and output.
* There are five kinds of data:
   * Input parameters ( altering these is a side effect )
   * Returned values ( from the function and out parameters )
   * Local variables
   * Global variables ( altering these is a side effect )

 **Keep your connections simple.**

 **Never cause side effects if you can avoid it**

 **Testing, Coding, and further Refinement**

 * Topdown Refinement uses stubs, whcih are empty functions.
 * *Sentinel* is an entry put into a data structure so that boundary conditions need not be treat as special cases.


**Input & Output**

* keep your input & output as separate function so they can be changed easily.

**Principles of Testing**

* The quality of test data is more significan than the quantity.
* Testing can demostrates the presence of bugs but never their absence.
* You are just building a level of confidences with testing.


**Methods of chosing data**

* Black box
  * Easy values - values that are easy to check
  * Typical realistic values - representative of how the program will be used.
  * Extreme values - limits of the range of applicatoin
  * Illegal values - How do these cases get handled

* Glass box testing
  * A program can not be considered tested if there are some paths of the code that have never been executed.
  * Chose data that will execute every path
  * Use in isolation against small modules
  * TDD is an application of Glass box testing.

## Pointers and Pitfalls

1. Be sure you understand your problem before you decide to solve it.
2. Be sure you understand the algorithm before you start to program.
3. In case of difficulty divide the problem into pieces and think of each part separately.
4. Keep your functions short and simple.
5. Include careful documentation with each function you write.
6. Write precise pre and post conditions for every function.
7. Include error checking at the begining of functions to check pre conditions.
8. Each time you use a function ask yourself what are its preconditions.
9. Use stubs, drivers, black box and glass box testing to simplify debugging.
10. Be wary of edge conditions.
11. Keep your code well formatted.
12. Keep your documentation consistent.
13. Explain your program to someone else.