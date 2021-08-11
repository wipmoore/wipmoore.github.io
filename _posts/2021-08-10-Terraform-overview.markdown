---
layout: post
title: "Terraform Overview"
date: 2021-08-10 15:00:00 +0100
categories:
---



- [Infrastructure as Code (IaC)](#infrastructure-as-code)
- [How does Terraform fit](#how-does-terraform-fit)
- [Terraform Basics](#terraform-basics)

## Infrastructure as Code (IaC)

Infrastructure as Code is where you describe your infrastructure using a declarative human readable language stored in configuration files.

This allows you to create a blueprint for your infrastructure that you can:

- Version
- Share
- Reuse

This provides the benefits of a systems infrastructure being:

- managed via configuration files
- able to consitently created
- versions
- described
- able to create multiple version of the infrastructure

### Infrastructure Lifecycle

- Day 0
- Day 1


#### Day 0

Code to provision and configure your initial infrastructure.

#### Day 1

OS and application configuration you apply after you have initialy built your infrastructure.


## How does Terraform fit

Terraform is an IaC tool that provides the following:

- It's own declarative definition language
- The ability to target multiple platforms
- State management


## Terraform Basics

HashiCorp defines the followin Terraform workflow:

- __Scope__ Identification of the infrastructure needed for a project.
- __Autor__ Write the configuration fot the infrastructure.
- __Initialize__ Install the plugins Terraform needs to manage the infrastrcuture.
- __Plan__ Preview changes terraform will make to match the configuration.
- __Apply__ Make the planned changes.

### Providers 

Are pluggins that allow terraform to interact with cloud platforms and service via their api


```

```

