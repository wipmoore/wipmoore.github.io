---
layout: post
title:  "Ninject and WebApi"
date:   2016-11-27 19:56:00 +0100
categories: jekyll update
---

To use Ninject as the dependecy resolver for WebApi projects you need to:

1. Install the following packages:

* Ninject
* Ninject.Common
* Ninject.Common.WebHost
* Ninject.WebApi


2. Modify the _NinjectWebCommon.CreateKernel_ method and register an NinjectDependencyResolver as the default dependency resolver. 

```
RegisterServices(kernel);
GlobalConfiguration.Configuration.DependencyResolver = new NinjectDependencyResolver(kernel);
```

_Note_ NinjectWebCommon is created in the App_Start folder.

# References

* http://nodogmablog.bryanhogan.net/2016/04/web-api-2-and-ninject-how-to-make-them-work-together 