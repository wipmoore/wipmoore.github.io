---
layout: post
title:  "RESTful API design notes"
date:   2016-09-24 11:57:21 +0100
categories: jekyll update
---
**Notes** on RESTful API design taken from various sources.

* Be RESTful
* Keep it simple
* Two URLs per resource
* Hide complex variation behind the query string
* Errors conditions are identified by status codes
* Always version
* Format should be an extension type on the resource
* Allow selectivity via the query string
* Search via the query string
* Pagination via the query string
* Attribute name should be camel case
* Non resource URLs should be verbs


# Be RESTful
Honour verbs:

* GET - get a resource
* POST - creates a resource
* PUT - updates a resource
* DELETE - deletes a resource



# Keep it simple

Verify with the following question, "If you cut and paste the url into a browser does something meaningful occurs?"

Use nouns for resource names.



# Two URLs per resource

<table>

  <thead>
    <tr>
      <td></td>
      <td>POST</td>
      <td>GET</td>
      <td>PUT</td>
      <td>DELETE</td>
    </tr>
  </thead>

  <tbody>

    <tr>
      <th>dogs</th>
      <td>create new dog</td>
      <td>list of dogs</td>
      <td>N/A</td>
      <td>all dogs</td>
    </tr>

    <tr>
      <th>dog</th>
      <td>N/A</td>
      <td>get dog's details</td>
      <td>replace dog</td>
      <td>delete dog</td>
    </tr>

  </tbody>
</table>
<br>



# Hide complex variation behind the query string

e.g. get all dogs that are red:

GET /v1/dogs?colour=red



# Errors conditions are identified by status codes

Error should be indetified by the http status code.

It is also helpful to send a json body

```
{
  "status"  : "401",
  "message" : "Identical dog already exists!",
  "type"   : {
    "code" :"<internal code that identifies the error type>",
    "more_info" : "<url where you can get more info on this type of error"
  },
  "error" : {
    "id": "<Unique id for the instance of this error that can be used if reporting the error",
    "more_info" : "<url where you can get more information"
  }
}
```


# Always version

All api's should have a version number and it should be the highest level of granularity.



# Format should be an extension type on the resource

if you are going to support multiple format this should be defined as an extension type on the resource

e.g.

GET /v1/dogs.json
GET /v1/dog/12345.json



# Allow selectivity via the query string

e.g. if you only want the name and breed of the dogs

GET /v1/dogs?field=id,name,breed



# Search via the query string

e.g. dogs that have fluffy fur and are red

GET /v1/dogs/search?q=fluffy+fur,colour+red

or for free text search

GET /v1/dogs/search?q=fluffy,red



# Pagination via the query string

e.g. I want 25 dogs on the page stating from the 50 dogs

GET /v1/dogs?limit=25&offset=50



# Attribute name should be camel case

Because that is the javascript naming convention and the majority of the time you are sending json to be converted into a javascript object.



# Non resource URLs should be verbs

When you have a url that is more algorithmic rather than resource ( i.e. behaviour over state ) use a verb.

e.g.

/v1/covert?from=EUR&to=GBP&amount=10

# Reference

<a href='https://en.wikipedia.org/wiki/Representational_state_transfer'>wikipedia</a>

<a href='https://www.youtube.com/watch?v=QpAhXa12xvU'>apigee
</a>
