---
layout: post
title: "Terraform Large Systems Organisation"
date: 2021-08-15 11:00:00 +0100
categories:
# Terraform Large Systems Organisation

- [Terraform Components](#terraform-components)
	- [State](#state)
	- [Workspaces](#workspaces)
- [Large System Organisation](#large-system-organisation)
- [Project Structure](#project-structure)

## Terraform Components

Description of the terraform components that are usefull when dealing with organisation for large systems.

### State

Terraform stores information about the infrastructure that it has created in a state file. By default name __terraform.tfstate__.

Terraform supports __remote backends__ which resolve the above three issues:

- Once configured Terraform will automatically load state from the remote backend.
- Most backends support locking which enables synchronization.
- Most backends support encryption at rest and in transmission which helps keep any secrets stored in the state confidential.

### Workspaces

State is stored in named workspaces the default namespace is named _default_

```
# What is the current workspace 
# What worksapces are available
# Create a new workspace called temporary 
# Change into a workspace


terraform workspace show
terraform workspace list
terraform workspace new booking-service-testing
terraform workspace select default
```

## Large System Organisation

This section covers the concerns that need to be addressed when organising for large systems and how they may be addressed.

In order to control the introduction of changes and be able to verify that a changes performs as expected a change will progress through different environment where each environment intended to either verify one or more effect that the change has on the system, demonstrate a feature to an interested party, introduce the change into the live system.

There are also various concerns that need to be addressed to facilitate concurrent development on a system.

### Environments

There are two distinct aspects regarding environments:

- A definition
- An instances of an definition

#### Environment Definition

With the introduction of Infrastructure as code (IaC) this is the collection of declarative definition files used to describe the infrastructure components of a system. For a system there may be multiple environment definitions which can be used to suite different purposes.

Components that are available to be used in a definition will always have the potential to be used in multiple definitions.

#### Environment Instance

An instance of an environment is deployed artifacts created from an __Environment Definition__.  There may be multiple instances that have been created from the same definition all serving a different purpose. 

### Concurrent Development Concerns

For projects that have multiple components with multiple teams working concurrently on these components there are the following concerns:

- Synchronization
- Isolation

#### Synchronization

When you have concurrent work being performed on a single system definition the ability to synchtonize work is needed. Regarding synchronization there are the following concerns that need to be addressed:

- Secure storage of state so that secrets are not leaked.
- Consistency and durability of the changes.

#### Isolation

The point of having different enviroments is that changes made only affect the environments they have been introduced to in a controlled manner. There following aspects need to be considered in regard to isolation:

- The abilty to control when a change is introduced to an instance of an environment.
- The abilty to bring instances of an environment definition.

## Project Structure

- Rquirements
  - Be able to provision and deploy an instance of a new environment
  - Be able to update components in a controlled way
  - Be able to provision new components into an existing instances of an environment

- Organisation is in two main parts
  - Components
  - Environment definitions

### Components

Are modules that define components that are reusable across environments and are stored in their own git repository.

### Environment Definitions

Use a combination of their definition specific components and the shared componenent modules to create an environment.

### Examples Structure

- stage
  - definition
    - main.tf
	- variables.tf
	- outputs.tf
  - data
  - network
  - services
    - frontend services
	- backend services

- prod
  - N. Repeat of stage but with components needed for a production environment

- mgmt
  - Jenkins
  - N. This is for Development infrastrcuture

- global
  - IAM
  - N. For things that are used by all environments

- N. Under services you would define compute resources.
- N. Each module would be its own repository
- N. Each product would be its own module defining: KeyVaults, storage, observability etc.


## References

- [Terraform state management](https://blog.gruntwork.io/how-to-manage-terraform-state-28f5697e68fa#.r6xdvtxqe)
- [Terraform reusable moduels](https://blog.gruntwork.io/how-to-create-reusable-infrastructure-with-terraform-modules-25526d65f73d)
