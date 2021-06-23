---
layout: post
title: "Decomposition"
date: 2021-06-16 15:00:00 +0100
categories:
---

The effectiveness of a modularization is dependent upon the criteria used in dividing the system into modules.  Where a module is a _work assignment unit_.

Benefits of modularization:

- _Development time should be shortened_ as separate groups can work on each module _with little need for communication_.

- _Product flexibility should be increased_ changes should be able to be made in one module without changing others.

- _Comprehesibility should be increased_ as the system can be studied one module at a time with the result the system can be better designed because it was better understood.

__Note__  Given what we now know from _systems theory_ and _complexity theory_ this may be too much of a simplification.

## Governing principles

- Decomposition should be performed on the basis of information hiding.

The interface and definition of each module should be chosen to reveal as little as possible about its inner workings.

__Note__ If not careful this can lead to inefficiencies as it may result in elaborate calling sequences ( Chatty systems ) due to repeated switching between modules.

Decompose based on a list of difficult design decisions or decisions which are likely to change.

The connections between module is not just the interface that they communicate over and the data that they transfer but also the _assumptions_ which they make.

You can only make changes that do not violate the assumptions that other modules have made about it.