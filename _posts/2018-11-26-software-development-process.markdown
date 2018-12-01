---
layout: post
title:  "Software Development Process"
date:   2018-11-26 15:08:00 +0100
categories: jekyll update
---

An overview of a development process

- Create repository.
- Add basic documentation:
    - Readme.
    - Documentation directory structure.
- Identify the functionality:
    - Discovery:
        - Aim is to understand what needs to be built.
        - Data/Stories from the user needs analysis.
        - Design sprint for understanding how to present the journey.
        - Event storming with User's and domain experts for domain rules.
    - Define initial features:
        - Each feature will contain:
            - A description of what a is wanted why it is wanted, and who wants it. 
            - A description of the expected value this will provide.
            - A List of acceptance criteria.
                - Includes performance and security.
    - Define demonstration points:
        - A set of feature used together to demonstrate some aspect of the system.
        - It will use one or more feature.
        - It may include already built features but should include at least one new feature.
        - Each demonstration point should have a list of the journies that make up the demonstration point.
    - Chose the Demonstration point(s) to work on:
        - Possibly multiple if there are more developer than needed for a single demonstartion point.
        - Note there should on average be an equal mix of Front office, Back office and infrastructure work.
- Develop
    - Bootstrap the project:
        - Deployment pipleine to delivery.
        - Test projects:
            - Acceptance, Automated test for demonstration points.
            - Performance, Automated test for data volume, load and stress.
            - Function, Happy,Sad,abuse and performace cases/scenarios for a feature.
            - Unit, Technical artifact for the development team.
        - Build pipeline.
    - Build demonstration point:
        - Flesh out the journies ( Ux and domain model ).
        - Create the Acceptance test for the journies.
        - Build a feature:
            - Flesh out the scenarios (QA,Dev,Subject matter expert).
            - Create the Functional tests for the feature.
            - Create the security tests for the feature.
            - Code the feature.
            - Create the performance tests for the feature.
        - Once all the features for the Demonstration point has been built:
            - All test should be green.
            - It should be manually demonstrated to the stakeholders.


N. Demonstration point are a unit of release.
N. The reason for the mix of Front office,Back office and infrastructure work is to ensure that the product can continue to be built in a sustainable way.  If you ignore any one of these aspects you are unlikely to be able to continue to build at stable rate. Infrastructure covers a multitude of aspects such as pipelines and tooling through to system architecture.
N. Performance test should cover:
    - Volume, large dataset already in the system and submitted to the system via requests
    - Load, Building from the volume this is high numbers of requests for a sustained periods.
    - Stress, Higher than planned for requests across the entire system.
