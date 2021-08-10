---
layout: post
title: "Asp.Net Core"
date: 2021-06-16 15:00:00 +0100
categories:
---

- [Environments](#environment)
- [Pipeline](#pipeline)
- [Authorization](#authorization)
- [Authentication](#authentication)
- [Configuration](#configuration)
- [Logging](#logging)
- [Hosts](#hosts)

## Environments

asp.net core reads the following environment variables to determine the environment and set IHostEnvironment.EnvironmentName:

- DOTNET_ENVIRONMENT
- ASPNETCORE__ENVIRONMENT

**Note**. ASPNETCORE_ENVIRONMENT overrides DOTNET_ENVIRONMENT


The following values have significance within asp.net core:

- Development
- Staging
- Production

**Note**. Production is the default if DOTNET_ENVIRONMENT or ASPNETCORE_ENVIRONMENT has not been set.


```c#
public class Startup
{
    private readonly IConfiguration _config;
    private readonly IWebHostEnvironment _env;

    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        _config  = configuration;
        _env     = env;
    }


    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            Console.WriteLine(_env.EnvironmentName);
        }
        ...
    }
    ...
}
```


## Pipeline

The pipeline is where you define what processing should be performed on each type of request.

```
GET /hello.htm HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
Host: www.tutorialspoint.com
Accept-Language: en-us
Accept-Encoding: gzip, deflate
Connection: Keep-Alive
```

```
HTTP/1.1 200 OK
Date: Thu, 10 Jun 2021 05:59:09 GMT
Expires: -1
Cache-Control: private, max-age=0
Content-Type: text/html; charset=ISO-8859-1
Server: gws
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN
Accept-Ranges: none
Vary: Accept-Encoding
Transfer-Encoding: chunked

Hello World
```

The pipeline is defined in _Startup.cs_ and consists of the following components:

- [Definition](#definition)
- [Middleware](#middleware)
- [Routing](#routing)
- [Endpoints](#endpoints)
- [Action invocation pipeline](#action-invocation-pipeline)

### Definition

The pipeline and services that it depends on are configured in Startup.cs.  The following are defined:

- Services that can be supplied via dependency injection
- The pipeline that will process requests

| Method            |                                               |
|------------------ |---------------------------------------------- |
| ConfigureServices | to configure the dependencies                 |
| Configure         | to configure the request processing pipeline  |

```c#
public class Startup
{
    IConfiguration configuration;

    public Startup( IConfiguration configuration )
    {
        this.configuration = configuration;
    }

    public void ConfigurServices( IServiceCollection  services )
    {
        ...
    }

    public void Configure( IApplicationBuilder app, IWebHostEnvironment env )
    {
        ...
    }
}
```

#### IApplicationBuilder

The asp.net core request pipeline consists of a sequence of request delegates that are called one after the other.  Each delegate can perform an action before and after the next delegate.  The pipeline is defined using the _IApplicationBuilder_ in the _Configure_ method of the _Startup_ class.

```c#
public class Startup
{
    ...

    public void Config(IApplicationBuilder app, IIWebHostEnvironment env)
    {
        ...
        app.UseAuthorization()
        ...
    }
}

```

#### IWebHostEnvironment

Implements :

- IHostEnvironment

  - WebRootFileProvide         ,An IFileProvider pointing at WebRootPath.
  - WebRootPath                ,The absolute path to the directory that contains the web-servable application content files.

- IHostingEnvironment:

  - ApplicationName
  - ContentRootFileProvider  ,An IFileProvider pointing at ContentRootPath.
  - ContentRootPath          ,The absolute path to the directory that contains the application content files.
  - EnvironmentName          ,The name of the environment.
  - IsDevelopment            ,Checks if the current host environment name is Development.
  - IsEnvironment            ,Compares the current host environment name against the specified value.
  - IsProduction             ,Checks if the current host environment name is Production.
  - IsStaging                ,Checks if the current host environment name is Staging.

#### Services

- Are defined in the _IServiceCollection_ which is passed to the _ConfigureServices_ method

- There are the following scopes:

|Scope     | Description                                                             |
---------- | ----------------------------------------------------------------------- |
|Scoped    | Different object for each request but the same object within a request. |
|Singleton | Same object for every request.                                          |
|Transient | Always supplies a new object.                                           |

__Note__. The container will call dispose on any services that it creates the implements IDisposable.

```c#
    public void ConfigureServices( IServiceCollection services )
    {
        services.AddScoped<IFileStorageService, AzureStorageService>();
        services.AddSingleton<Service2>();
        services.AddTransient<Service1>();

        var myKey = Configuration["MyKey"];
        services.AddSingleton<IService3>(sp => new Service3(myKey));
    }
```

### Middleware

Middleware is the heart of an asp.net application, its purpose is to allow small composable pieces of functionality that perform some action on all requests.  There are two variants of middleware: delegate based and class based with the two main constituents of a middleware component being:

- [context :httpcontext](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.http.httpcontext?view=aspnetcore-5.0)
- [next :requestdelegate](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.http.requestdelegate?view=aspnetcore-5.0)


```c#
public delegate Task RequestDelegate(HttpContext context);
```

```c#
app.Use( async (context, next) => {
    IMessageFormatter formatter = context.RequestServices.GetService<IMessageFormatter>()

    await context.Response.WriteAsync(formatter.Format("M1.1"));
    await next.Invoke();
    await context.Response.WriteAsync(formatter.Format("M1.2"));
});
```

```c#
app.Run( async (context) => {
    IMessageFormatter formatter = context.RequestServices.GetService<IMessageFormatter>();

    await context.Response.WriteAsync(formatter.Format("M2.1"));
});
```

```c#
public class RequestCultureMiddleware
{
    private readonly RequestDelegate _next;

    public RequestCultureMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var cultureQuery = context.Request.Query["culture"];
        if (!string.IsNullOrWhiteSpace(cultureQuery))
        {
            var culture = new CultureInfo(cultureQuery);

            CultureInfo.CurrentCulture    = culture;
            CultureInfo.CurrentUICulture  = culture;
        }

        // Call the next delegate/middleware in the pipeline
        await _next(context);
    }
}

...

public class Startup
{
    ...

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        ...
        app.UseMiddleware<RequestCultureMiddleware>();
    }
}
```

__Note__. The order in which the middleware is added to the pipeline matters as preceding middleware is responsible for adding data to the context and deciding if to continue or terminate the pipeline where the order is the order in which it is added in the _Configure_ method.


### Routing

Routing is responsible for dispatching HTTP request to an _endpoint_ for processing.  Endpoints are defined within an application's and configured when the app starts.  The endpoint matching process can extract values from the request's URL and provide those values for request processing.  Routing is also able to generate URLs that map to endpoints.

Routing uses a pair of middleware, registered by UseRouting and UseEndpoints:

- UseRouting adds route matching to the middleware pipeline.
- UseEndpoints adds endpoint execution to the middleware pipeline.


```c#
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", async context =>
        {
            await context.Response.WriteAsync("Hello World!");
        });
    });
}
```

An endpoint is something that can be:

- Selected, by matching the URL and HTTP method.
- Executed, by running the delegate.
- Endpoints are configured in UseEndpoints that connection request delegates to the routing system.  Method are:
  - MapGet
  - MapPost
  - etc

```c#
app.UseEndpoints(endpoints =>
{
    endpoints.MapGet("/hello/{name:alpha}", async context =>
    {
        var name = context.Request.RouteValues["name"];
        await context.Response.WriteAsync($"Hello {name}!");
    });
});
```
Route templates such as _/hello/{name:alpha}_ are used to configure how the endpoint are matched. With this example:

- Any URL path that begins with /hello/ followed by a sequence of alphabetic characters is matched.
- The second segment is captured and stored in the _HttpRequest.RouteValues_

### Endpoints

Routing extends the middleware concept by adding the concept of an _endpoint_ which represent units of an applications functionality that are distinct from each other in terms of routing, authorization, etc.

An ASP.NET Core endpoint is:

- Executable: Has a RequestDelegate.
- Extensible: Has a Metadata collection.
- Selectable: Optionally, has routing information.
- Enumerable: The collection of endpoints can be listed by retrieving the EndpointDataSource from DI.

```c#
// MVC endpoint example;
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
        services.AddControllers();
        ...
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        ...
        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
        ...
    }
```

### Action invocation pipeline

Where the middleware pipeline is the same for each request the 'action invocation pipeline' varies based on the resource that is being requested ( mvc endpoint selection ).  The pipeline is matched based on the request url against the the route template.

The action invocation pipeline has the following stages some of which a request will go through twice:

1. [Authorization Filters (_in_)](#1-authorization-filters)
2. [Resource Filters (_in_)](#2-resource-filters)
3. [Model binding and Validation (_in_)](#3-model-binding-and-validation)
4. [Action Filters (_in_)](#4-action-filters)
5. [Action Execution](#5-action-execution)
6. [Exception Filters (_out_)](#6-exception-filters)
7. [Result Filters (_out_)](#7-result-filters)
8. [Resource Filters (_out_)](1-resource-filters)
9. [Result Execution (_out_)](#9-result-execution)

Filters can be applied at any of the following scopes:

1. _Global_, Applies to all actions
2. _Controller_, Applies to all actions defined in the controller
3. _Action_, Applies to just that action

__Note__ the evaluation order is from the wides scope ( _Global_ ) to the narrowest ( _Action_ )

Filters are intended to allow cross cutting concerns to be applied across multiple request specific 'action invocation pipelines'.  They also have access to more data than middleware in that they can also access:

- RoutingData
- Model state
- Result

[Microsoft.AspNetCore.Mvc.Filters](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters?view=aspnetcore-5.0)

### 1. Authorization Filters

Authorization filters are run first and are used to determine whether the user is authorized for the request.  Authorization filters short-circuit the pipeline and return the result without reaching the next stage if the user is not authorized.

Exceptions that are raised by an authorization filter will not be handled by the exception filter.

A description of the common type of authorization supported in the mvc framework can be found in the [Authorization section](#authorization)

- [IAsyncAuthorizationFilter](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncauthorizationfilter?view=aspnetcore-5.0)

### 2. Resource Filters

Resource Filters are invoked immediately after the authorization filters in the pipeline. They can short-circuit the pipeline and are commonly used to access responses from a cache or to verify that the http request has headers that are specific to the resource that it is to be applied to, etc.

Resource filters execute before model binding so you can handle binding for the specific requests.

- [IAsyncResourceFilter](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresourcefilter?view=aspnetcore-5.0)

A resource filter context has access to the following:

- Result
- ValueProviderFactories
- Filters
- ActionDescriptor
- HttpContext
- ModelState
- RouteData

### 3. Model binding and Validation

Is the creation and population of a model or models from the request. The following are targets for model binding:

- Parameters of the controller action method that a request is routed to.
- Public properties of a controller if specified by attributes [BindProperty] .

__Note__ By default properties are not bound for HTTP Get request to bind on a Get request you have to specify SupportsGet on the BinProperty attribute.

```c#
[BindProperty(Name = "ai_user", SupportsGet = true)]
public string ApplicationInsightsCookie { get; set; }
```

The data used to populate the models will by default be key-value pairs from the following sources in a HTTP request and searched in the following order:

- Form fields
- The request body (For controllers that have the [ApiController] attribute.)
- Route data
- Query string parameters
- Uploaded files

There are a few exceptions:

- Route data and query string values are used only for simple types.
- Uploaded files are bound only to target types that implement IFormFile or IEnumerable<IFormFile>.

Any of the following attributes can be applied to override the defaults:

| Attribute          |                                                         |
|------------------- |-------------------------------------------------------- |
| [BindRequired]     | Model binder must bind the value if a value can not be found an error is added to the Model state. |
| [BindNever]        | The model binder should ignore the binding of this property. |
| [FromQuery]        | Gets values from the query string. |
| [FromRoute]        | Gets values from route data. |
| [FromForm]         | Gets values from posted form fields.  |
| [FromBody]         | Gets values from the request body.  |
| [FromHeader]       | Gets values from HTTP headers.  |

You can specify a name for the of the field to populate the property from in the attribute:

```c#
public class MakeBookingRawRequest
{
    public int PropertyId { get; set; }

    [FromQuery(Name = "Note")]
    public string NoteFromBookingRequest { get; set; }

    ...

}

[ApiController]
public class BookingsByCountryController
{
    ...

    public void Get([FromHeader(Name = "Booking-Country")] string countryCode)
    {
        ...
    }

    ....

}
```

#### FromBody Attribute

Asp.net core runtime delegates the responsibility of reading the body to an input formatter.  When the _FromBody_ is applied to a complex type parameter any binding source attributes applied to its properties are ignored.

The FromQuery binding source attribute on the LastName property is ignored because the customer property of the Get action is ascribed with the FromBody binding source attribute because Input formatters only read the body and do not understand binding source attributes.

```c#
public class CustomerBookingsByNameRequestRaw
{
    public int FirstName { get; set; }

    [FromQuery(Name = "LastName")]
    public string LastName { get; set; }

    ...

}

[ApiController]
public class CustomerBookingsByNameController
{
    ...
    public void Get([FromBody] CustomerBookingsByNameRequestRaw customer)
    {
        ...
    }

    ....

}
```

The __FromBody__ binding source attribute should only be applied to a single parameter of an action method as once the request stream has been read by an input formatter it is no longer available to be read again.


After each property is successfully bound model validation occurs for that property. The record of what data is bound to the model and any binding or validation errors is stored in ControllerBase.ModelState which will update the ModelState.IsValid flag.

#### Value providers

Source data for the model binding system is provided by value providers.  To create and register an additional provider:

- Create a class that implements IValueProvider.
- Create a class that implements IValueProviderFactory.
- Register the factory class in Startup.ConfigureServices.

To register the provider to be used after all the other providers:
```c#
services
    .AddMvcOptions(options =>
    {
        options.ValueProviderFactories.Add(new CookieValueProviderFactory());
    })
```

To add the provider as the first provider to be used.
```c#
services
    .AddMvcOptions(options =>
    {
        options.ValueProviderFactories.Insert(0, new CookieValueProviderFactory());
    })
```

#### Model properties that have no source

It is not considered a model state error if a binding source can not be found for a model property it will just be set to null or its default value.

- Non nullable types are set to __default(T)__
- Complex types a instance is created using the default constructor without setting any properties.
- Arrays are set to Array.Empty<T>() (byte[] are set to null)


#### Type conversion errors

If a binding source is found but can not be converted to the target type the model state is flagged as invalid and the target parameter or property is set to null or default.

In an API controller that has the [ApiController] attribute an invalid model start results in an authomatic HTTP 400 response.

```c#
public IActionResult Post()
{
    if (!ModelState.IsValid)
    {
        return BadRequest();
    }
    ...
}
```

#### Simple types

Simple types that the model binder can convert source strings into include the following:

- Boolean
- Byte, SByte
- Char
- DateTime
- DateTimeOffset
- Decimal
- Double
- Enum
- Guid
- Int16, Int32, Int64
- Single
- TimeSpan
- UInt16, UInt32, UInt64
- Uri
- Version

#### Complex types

Complex types must have a public default constructor and public writable properties to bind. For each property of the of the complex type model binding looks through the binding sources for the _name pattern prefix.property name__.  If a match is not found it looks for just the _property name_ without the prefix.

- For parameters the prefix is the parameter name.
- For complex properties the prefix is the property name.

e.g. The First name will be matched to a source in the request labeled customer.FirstName
```c#
public class CustomerBookingsByNameRequestRaw
{
    public int FirstName { get; set; }

    [FromQuery(Name = "LastName")]
    public string LastName { get; set; }

    ...

}

[ApiController]
public class CustomerBookingsByNameController
{
    ...
    public void Get([FromBody] CustomerBookingsByNameRequestRaw customer)
    {
        ...
    }

    ....

}
```

#### Custom prefix

The prefix that is expected in the binding sources can be changed using the __Bind__ attribute.

```
public IActionResult Post([Bind(Prefix="Customer")]CustomerBookingsByNameRequestRaw customerRequest )
```

#### Collections

Collections of simple types model binding matches to parameter name or property name

For targets that are collections of simple types, model binding looks for matches to parameter_name or property_name. If no match is found, it looks for one of the supported formats without the prefix. For example:

```
public IActionResult OnPost(int? id, int[] selectedCourses)


// selectedCourses=1050&selectedCourses=2000
// selectedCourses[0]=1050&selectedCourses[1]=2000
```

Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero

#### Dictionaries

For targets that are dictionaries of simple types, model binding looks for matches to parameter_name or property_name. If no match is found, it looks for one of the supported formats without the prefix.

```c#
public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)

// selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
// selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
// selectedCourses["1050"]="Chemistry"
// selectedCourses["2000"]="Economics"
```

#### Records

Model binding to record types with a single constructor is supported

```c#
public record Person([Required] string Name, [Range(0, 150)] int Age, [BindNever] int Id);

public class PersonController
{

   [HttpPost]
   public IActionResult Index(Person person)
   {
       ...
   }
}
```

#### Model state

Model state is derived from:

- Model binding state
- Model validation state

Errors from __model binding__ are generally __data conversion errors__ where as errors from __validation__ are more likely to be __business rules__.

#### Model Validation

Occurs after model binding and reports errors where data does not conform to business rules. The outcome of model validation is reported in the _ModelState.IsValid_

```c#
public async Task<IActionResult> OnPostAsync( BookingDetailsRequest request )
{
    if (!ModelState.IsValid)
    {
        // rfc 7808 Problem details
        return new ObjectResult( new ValidataionProblemDetails( ModelState )
        {
            Detail = "Validation Errors in Request",
            Status = 400,
        });
    }
    ...
}
```

##### Automatic 400 response on Validation failure

If the controller is ascribed with an __ApiController__ attribute if it fails model validation it will automatically respond with a 400 error and never reach the action. This can be __disabled__ by:

```c#

public void ConfigureServices( IServiceCollection services )
{
    ...

    services
        .AddControllers()
        .ConfigureApiBehaviorOptions(options =>
        {
            ...
            options.SuppressModelStateInvalidFilter = true;
            ...
    });

}
```

#### Repeat validation

If a request fails validation you can modify it and try again.

```c#
public async Task<IActionResult> Post( BookingRequestRaw request )
{
    if(!ModelState.IsValid)
    {
        request.SubmittedDate = DateTime.UtdNow;

        ModelState.ClearValidationState(nameof(request)));
        if (!TryValidateModel(request, nameof(request)))
        {
            return new ObjectResult( new ValidationProblemDetails( ModelState )
            {
                Detail = "Request failed validation",
                Status = 400,
            });
        }
    }
    ...
}
```

Domain specific attributes can be create:

```c#
[AttributeUsage(AttributeTargets.Field | AttributeTargets.Property)]
internal class ValidFromAttribute : ValidationAttribute
{
    DateTime minDate;

    public ValidFromAttribute
             ( DateTime minDate = default )
    {
        this.minDate = minDate != default ? minDate : DateTime.UtcNow.AddYears( -5 );
    }

    public override bool IsValid
                           ( object value
                           , ValidationContext context )
    {
        var request   = (BookingRequest)validationContext.ObjectInstance;
        var from      = ((DateTime)value).Year;

        return from.Year >= minDate.Year &&  from < request.To;
    }

    public override string FormatErrorMessage(string name)
        => $"{name} is not valid, it must be on or after {minDate.Year} and before the bookings to date.";
}
```

Model consistency validation can be applied via the __IValidatableObject__ interface:


```c#
public class BookingRequest : IValidatableObject
{
    ...

    public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
    {
        var request   = (BookingRequest)validationContext.ObjectInstance;

        if ((request.To - request.From).TotalDays < 3)
        {
            yield return new ValidationResult(
                "Invalid date range specified, must be more at least three days."
                new[] { nameof(ReleaseDate) });
        }
    }
}
```

#### Automatic short-circuit

Validation is automatically short-circuited (skipped) if the model graph doesn't require validation. Objects that the runtime skips validation for include collections of primitives (such as byte[], string[], Dictionary<string, string>) and complex object graphs that don't have any validators.


### 4. Action Filters

Action filters are invoked after model binding before and after action execution so they can be used to modify the model and the result. Action filters implement the _IAsyncActionFilter_.

- [IAsyncActionFilter](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncactionfilter?view=aspnetcore-5.0)

An action filter context has access to the following:

- Controller
- Result
- ActionArguments
- Filters
- ActionDescriptor
- HttpContext
- ModelState
- RouteData

_Note_ A typical use for these is adding cache control headers to the response.  As cache control is specific to each request it is a better fit to apply it at the _action invocation pipeline_ stage that in middleware which is applicable to all requests.

### 5. Action Execution

Action execution is where the target action method is invoked.

### 6. Exception Filters

Exception filters apply global policies to unhandled exceptions that occur before the response body has been written to.  These filters are invoked for unhandled exceptions in:

- Controller creation
- Model binding
- Action filters
- Action methods

They are not invoked from unhandled exceptions in:

- Authorization filters
- Resource filters
- Result filters
- Result Execution

[IAsyncExceptionFilter](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncexceptionfilter?view=aspnetcore-5.0)

An action filter context has access to the following:

- Exception
- Result
- ExceptionHandled ( bool )
- ExceptionDispatchInfo
- Filters
- ActionDescriptor
- HttpContext
- ModelState
- RouteData

Setting ExceptionHandled to true or writing a response will stop propagation of the exception.

In general middleware should be used to handle exceptions with this being just for one off handling of exceptions for specific actions.

### 6. Result Filters

Result filters surround the execution of action results.  They are only executed when an _action_ or _actions filter produces a result_.

[IAsyncResultFilter](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter?view=aspnetcore-5.0)

## Authentication

There are two parts to the authentication in an asp.net application:

- Challenge
- Authentication

### Challenge

Validates security credentials this is now commonly an OpenIdConnect or OAuth2 Idetity provider.

## Authentication

Serializes/Deserializes a verified user.  Depending on the the type of application API or client this will usually differ:

- __API__ commonly use JWT presented as a Bearer token from an OpenIdConnect or OAuth2 Identity provider.
- __Client__ commonly use Cookie to persist user information provided by the Challenge scheme.

The outcome of the Challenge or Authentication subsystem is the population of the __User__ object on the __HttpContext__. This is a __ClaimsPrincipal__ that contains a record of the claims were reported for the authenticated user.



## Authorization

## Configuration

Configuration in asp.net core is loaded using one of more configuration providers.  Configuration providers read configuration data from an underlying source:

- appsettings.json
- Environment variables
- Azure Key Vault
- Azure App Configuration
- Command-line arguments

The providers that are used are defined by the hosts as configuration is an environmental concern.  Providers are layered over each other and a provider that contains the same setting as a provider that has already been loaded will override any setting from a previously loaded provider.

The default webhost uses the following providers:

- appsettings.json
- appsettings.Environment.json
- App secrets ( if running in the Development environment )
- Environment variables
- Command-line argument


## Logging

## Hosts

## References
