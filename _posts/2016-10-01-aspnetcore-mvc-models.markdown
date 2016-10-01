---
layout: post
title:  "ASP .Net Core MVC Models"
date:   2016-10-01 12:38:00 +0100
categories: jekyll update
---

* Model Binding
* Binding errors an model validation
* Model state


## Model Binding

* It occurs once a request has been rooted to a controller action.
* It is responsible for binding data from HTTP requests to the action method parameters.
* It tries to bind request data to the action parameters by name, in order from the following:
   * Form values
   * Route values
   * Query string

**Note** if the model data should come from the body which is not
x-www-url-formencoded then you need to specify that the model binder uses the
body explicitly with the [FromBody] attribute but you will need an
IInputFormatter that supports the input format.  If [FormBody] attribute is used
and post is x-www-url-formencoded then the model will not have an properties
bound as by default there is is not a formencoded IInputFormatters ( _Presumably you could add one_ ).

```
public class PersonController
              : Controller  {

    [HttpPost]
    public IActionResult Index
                          ( [FromBody] Person person ) {

        return Json(
          person
        );   
    }
}
```

Classes that are created as part of the default model binding must:

* Have a pubic default constructor
* All bound members must be publicly writable ( **Note**  need to check if they can be fields as we tried that once and it all went horribly wrong. )


### Customising model binding behaviour

* [BindRequired] - This attribute adds a model state error if binding can not occur.
* [BindNever] - Tells the model binder not to bind this property.
* [FromHeader]
* [FromQuery]
* [FromRoute]
* [FromForm] - Specifies the scope of a binding, i.e. where to get the data from.
* [FromService] - Dependency injection to bind parameter ( **Note** need to see how this works )
* [FromBody] - Use the configured formatters to bind the date from the request body. The formatter is selected based on content type.
* [ModelBinder] - Used to override the default model binder ( **Note** need to see how this works )


## Binding errors and model validation

Validation is performed as part of the model binding process.  There are three types of
errors:

* _Binding error_ missing data, non nullable fields not supplied.
* _Binding error_ incompatible data, e.g. a value that can not be converted to a date
* _Validation error_ where the supplied data does not conform to a validation attribute.

After the model binder has finished binding the data to the properties if at
least one value was bound to the model then all validators are run.  The model
model's metadata is used to identify the validators, any failed validation
rules are placed into the model state.

**Note** On a project with complex validation scenarios the Mvc
validation system did not seam to be sufficient. We needed to validate:

* that the fields in the request were valid in themselves.
* that the request was valid in itself ( e.g. if its a date range the start date is before the end date )
* that the request is valid against the context of the world.  ( e.g. you are allowed to apply that request to the world in its current state )

It was also required that any validation that could be performed was. So if you
a request that had a missing name property but the date range was valid you then
needed to see if the date range could have been applied to the world.  

As such we abandoned the standard model validation and accepted that all
validation was the responsibility of the service layer which returned a Result
monad.

The ValidationAttribute class can be used to create custom validation, you just
need to inherit from it and override the IsValid method.

```
...
using System.ComponentModel.DataAnnotations;
...

public class CustomAttribute
              : ValidationAttribute {

    private readonly string _other;

    public CustomAttribute
            ( string other ) {

        _other
         = other;
    }

    protected override ValidationResult IsValid
                                         ( object value
                                         , ValidationContext validationContext )
                                         {

        var property
             = validationContext
                .ObjectType
                .GetProperty(
                  _other
                 );

        if ( property == null ) {

            return new ValidationResult(
                string
                 .Format(
                     "Unknown property: {0}"
                    ,_other
                  )
            );
        }

        var otherValue
             = property
                .GetValue(
                   validationContext.ObjectInstance
                  ,null
                 );

        // at this stage you have "value" and "otherValue" pointing
        // to the value of the property on which this attribute
        // is applied and the value of the other property respectively
        // => you could do some checks
        if ( !object.Equals( value, otherValue ) ) {

            // here we are verifying whether the 2 values are equal
            // but you could do any custom validation you like
            return new ValidationResult(
               FormatErrorMessage(
                 validationContext
                  .DisplayName
               )
            );
        }
        return null;
    }
}
```

## Model state

Contains all the values that were bound to model properties as well as all the errors associated with with the model and its properties.

```
namespace Microsoft.AspNetCore.Mvc.ModelBinding {

    /// <summary>
    /// Represents the state of an attempt to bind values from an HTTP Request
    /// to an action method, which includes validation information.
    /// </summary>
    public class ModelStateDictionary
                  : IReadOnlyDictionary<string, ModelStateEntry> {

      ...

    }

}
```

```
namespace Microsoft.AspNetCore.Mvc.ModelBinding {

    /// <summary>
    /// An entry in a <see cref="ModelStateDictionary"/>.
    /// </summary>
    public abstract class ModelStateEntry { ... }

```

The model state is passed to the view so that it can access model values and
error messages etc.


## References


* <a href='https://docs.asp.net/en/latest/mvc/models/index.html'>asp.net core documentation</a>
* <a href='http://andrewlock.net/model-binding-json-posts-in-asp-net-core/'>Form body attribute post</a>
* <a href='http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html'>Brad wilson post on Input Validation vs Model Validation</a>
* <a href='https://books.google.co.uk/books?id=FaQLBAAAQBAJ&pg=PA147&lpg=PA147&dq=asp.net+mvc+does+validation+occur+as+part+of+binding&source=bl&ots=kHvCEJqmbQ&sig=XftvZ1FZKxGJsG3K4fGhxplqVUU&hl=en&sa=X&ved=0ahUKEwi48Oqf9rnPAhUILcAKHZNZBzoQ6AEIQjAG#v=onepage&q&f=false'>Professional Asp.Net Mvc 5</a>
