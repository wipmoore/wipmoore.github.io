---
layout: post
title:  "Javascript execution context"
date:   2016-12-08 20:42:00 +0100
categories: jekyll update
---

The javascript runtime environment has three concepts:

* Execution Stack
* Object Heap
* Message Queue


Javascript run in a single thread so only one thing can occur at a time, other action and events are queued for processing once the execution stack becomes empty.


## Exection stack

When a _context_ is to be executed an _execution context_ is created on the _stack_.  If the executing code makes a call to function then an _execution context_ for the called function is created and pushed onto the the _stack_.  Once the currently executing context completes its _execution context_ is popped of the stack.

There are three types of context:

* Global - code outside of any function.
* Function - code that is defined in a function.
* Eval - code from an eval statement.


### Execution context

This is a stack frame that conceptually contains three things:

* Variable object - the functions vars, function definitions, and function arguments
* Scope chain - a structure that references the functions ancestors "Variable objects"
* this object reference

Note - it is the _Scope chain_ that makes clojures work.

When an identifier is references the Variable object is checked first then it uses the Scope chain checking the most recent ancestor first.


## Object Heap

All objects are allocated in a heap which is just a name to denote a large mostly unstructured region of memory.


## Message Queue

This is a list of messages to be processed, each message has a function associated with it. When the _execution stack_ is empty a message is taken of the queue and processed. The processing consists of executing the messages associated function. 


# References

* https://developer.mozilla.org/en/docs/Web/JavaScript/EventLoop
* http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/