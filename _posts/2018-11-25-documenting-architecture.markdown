---
layout: post
title:  "Documenting Architecture"
date:   2018-11-25 15:08:00 +0100
categories: jekyll update
---

## Ubiquitous Language

- Containers, are units of execution like a docker container, azure web apps but could just as easily be a process.
- Components, are the artefacts that a container is made of.  There needs to be a close relationship between the models that are produced and the artefacts of the language that the containers are written in.

E.g. 

In C# you have the following artefacts:

- modules/files
- assemblies
- namespaces

So the component within a system built in C# should be created from these.

## Models 


There should be models at the following levels of abstraction: 

- System Context
- Container View 
- Component View

The abstractions represented in a model should not just be a box with a name they should contain enough contextual information that if the boxes and names were removed it would still provide enough information for someone to gain an understanding of the system.

Each of these models shows a higher level of abstraction than its successor. Any one level of abstraction should have as few models as possible as such any model for a level of abstraction will where possible include information that would be captured in multiple UML views. 

### System view

This view will delineate the boundary between the system and the other systems that it has to interact with.  The aim being to identify what the system is responsible for and what responsibilities it passed on to other systems.  A statement of the purpose of the system should be included in the model as well.

Q. What if anything is the difference between the way containers within a system communicate with each other as opposed to external systems?  In other words what is it that makes you distinguish between containers that are part of the system you are modeling and external systems.

N. There may be something in the reasons that we break thing into module namely to manage complexity and utility which breaks down into supporting our ability to be able to comprehend a system and our ability to be able to reuse common functionality.


### Container View
  
This view identifies the containers within a system and how they relate to each other and to any external systems they interact with.  Any bootstrap relationships will also be identified such as if environment settings are configured to come from a vault etc.  also items such as sharding, HA configuration etc. will be identified in this view.

### Component View

Describes the responsibilities and relationships between the components within a container where the components are namespaces, assemblies and modules.  Not every container will need one of these.  For a statefull container there should also be an ERD.
