---
layout: post
title:  "ASP .Net Core MVC Views"
date:   2016-09-24 23:36:00 +0100
categories: jekyll update
---

* Layouts
* partial views
* View components
* \_ViewStart.cshtml
* \_ViewImports.cshtml
* View discovery
* Passing data to views


# Layouts

Allow you to define a common layout for all views also used to define common scripts and css. By convention the layout is stored in a file called _\_Layout.cshtml_ in the _/Views/Shared_ folder.

The layout to apply to a view can be defined via the _Layout_ property.

```
@{
    Layout = "_Layout";
}
```
```
@{
    Layout = "/Views/Shared/_Layout.cshtml";
}
```


* @RenderBody()
* @RenderSection( <section name>, required: false)

*Note* Any view that has defined a layout with section in it is expected to supply all sections that are not optional.  An exception will be thrown if they are not specified.


* @IgnoreBody()
* @IgnoreSection(string sectionName)

Allow you to specify that you do not want to render the body or the section

```
@if (isAdmin) {
     <div>Admin layout <a href="#">sign out</a></div>
     <div>
         @RenderSection("adminSection")
         @IgnoreBody()
     </div>
 } else {
     <div>normal layout</div>
     <div>
         @IgnoreSection("adminSection")
         @RenderBody()
     </div>
 }
```

<a href='https://github.com/aspnet/Mvc/issues/3293'>Issue report that caused these features to be added</a>


# Partial views

A partial view is a view that is rendered within another view.  They are define within _.cshtml_ files, any view can be a partial view the only difference is that when the view is rendered as a partial view _\_ViewStart.cshtml_ is not called.

```
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

A partial gets a copy of the parent view’s ViewData dictionary, but this is just a copy updates made to the ViewData are not visible to parent.

```
@Html.Partial("PartialName", customViewData)
@Html.Partial("PartialName", viewModel)
@Html.Partial("PartialName", viewModel, customViewData)
```


# View components

View Components are intended for reusable rendering logic that is too complex for a partial view. A view component can be a POCO decorated with the [ViewComponent] attribute or derived from a class decorated with the [ViewComponent], there is also a _ViewComponent_ class which provides some useful methods and properties.

A view component class support constructor dependency injection. The _InvokeAsync_ method which returns an _IViewComponentResult_ is where the logic is defined.  Any argument are supplied directly as a parameters.

The runtime searches for the view in the following paths:

* Views/<controller_name>/Components/<view_component_name>/<view_name>
* Views/Shared/Components/<view_component_name>/<view_name>

There are the following methods for invoking a view component from a within a view:

* @Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
* @await Component.InvokeAsync("PriorityList", new { maxPriority = 2, isDone = false })


# \_ViewStart.cshtml

Used to define common view code that you execute at the start of each view rendering.  This lives in the Views folder.

It can be used to define a common layout to be applied to all views in this file.


# \_ViewImports.cshtml

Container for namespaces and directives that you want to import into all views. This lives in the Views folder.

The following directives are supported:

* @addTagHelper
* @removeTagHelper
* @tagHelperPrefix
* @using
* @model
* @inherits
* @inject  ( **Note** - not sure this is a good idea as you may end up building a god class. )



# View discovery

Unless a specific view filename is specified the runtime looks for a view

* a controller specific view
* a matching view name in the shared folder

1. Views/controller name/view name.cshtml
2. Views/Shared/view name.cshtml


```
public IActionResult Index() {
  ...
  return View();  // The action name is used as the view name.
}
```


```
public IActionResult Index() {
  ...
  return View( "LeagueTable" );  // The LeagueTable is used as the view name.
}
```


```
public IActionResult Index() {
  ...
  return View( "Views/Index.cshtml" ) // Path to the view file.
}

```

**Note** - The argument is the path to the view file.  The path needs to be the relative to the project root.


# Passing data to views


### Model

Provides a strongly typed model, this needs to be passed in as an argument when calling the View method in the controller.  It is actually stored in the ViewData.

It is accessed via @Model in a razor view.


### ViewData

Is a Dictionary that is passed into the view from the controller.


### Presentations

* [Asp.net Core Pipeline](/presentations/asp-net-core)
