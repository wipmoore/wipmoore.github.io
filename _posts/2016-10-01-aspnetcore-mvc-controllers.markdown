---
layout: post
title:  "ASP .Net Core MVC Controllers"
date:   2016-10-01 18:38:00 +0100
categories: jekyll update
---

## Controllers

Are used to define a set of one or more actions, where an action is a method on
a controller that handles incoming requests.

A controller can be any class:

* that's name ends with "Controller"
* that inherits from a class called name ends with "Controller"

**Note** _I tend to only have one action per controller, I apply a rule that says
that everything that is injected into the constructor of a class must be used
either directly or indirectly by every public method which generally results in
single action controllers._


Discovery:

* Any matching class located in the "Controllers" folder
* Any class Inherited from Microsoft.AspNetCore.Mvc.Controllers


## Asp.Net MVC

* The controller is responsible for initial processing of a request.
* Business decision should be performed within the a domain model.
* Accept the result of the domain model processing a request and return the
result proper view along with the associated view data.

**Note** _Personally I find it a bad idea for the view and domain model be the
same class.  They are dealing with different concerns so I consider it to be
good SOLID practice to have them as different clases._

_I prefer to provide access to the domain models via the command pattern._
