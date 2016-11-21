---
layout: post
title:  ".Net core on OSX"
date:   2016-11-21 11:57:21 +0100
categories: jekyll update
---

# Parts

* Runtime - contains the type system, assembly loading, garbage collection ( github.com/dotnet/coreclr )
* A set of frameworks and libraries ( github.com/dotnet/corefx )
* A set of tools e.g. language compilers etc. Known as the **SDK** ( github.com/dotnet/cli )
* Apphost selects the runtime to use for an assebly etc.


# File locations

* /usr/local/share/dotnet/sdk - contains the different version of the sdk
* /usr/local/share/dotnet/shared/Microsoft.NETCore.App - constains the diffrent version of the runtime and framework


# dotnet cli commands

* dotnet new - new C# or F# console application
* dotnet restore - restores dependencies
* dotnet build - builds the application
* dotnet publish - creates an xcopyable version of the app that you should be able to run via *dotnet {path to published assembly}*
* dotnet pack - creates an nuget package for the project
* dotnet --version - tells you what version of the sdk you are using

