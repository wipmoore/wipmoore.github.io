---
layout: post
title:  "OAuth2 and OpenId Connect"
date:   2016-10-22 15:48:00 +0100
categories: jekyll update
---

## Basics  

* Authentication - The process of identifying who you are e.g. verifying a username and password.
* Authorisation - The process of identifying what an identity can and can not do.

**Note** - Authorisation are the actions that you can apply plus the resouces that you can apply it to.  Just because you are authorised to perform a delete action does not mean you are authorised to apply that delete action to any resource.  In an enterprise system ( one where the users manipulate data about third parties e.g. a user is more likely to be manipulating data about other employees than themselves ) this appears to make verification of the right to perform an action not just a function of an access token ( i.e. the authorisation server ) but is also a domain function. i.e.  The authorisation server say what action an identity can and can not do but the domain decides what resources the identity can apply the actions to.  This is probaly quite differenct to the internet service providers which is what OAuth2 is aimed at.

## OAuth2

This is an *authorisation* protocol that is popular with web based services.

The central theam is about a **client** ( a.k.a. relying party ) getting an **access token** from an **authorisation server** ( a.k.a  identity provider ) that identifies what actions it can perform on a users data. There are different approaches ( a.k.a. flow ) to getting the access token, the apporach used will depend on the type of application that is asking for an access token, i.e. A different approach would be used betwen a single page application that spoke directly to an RESTful api to get it data than a more tradition web client that had a server that accessed the data.

**NOTE** - The terminology that people use in the reference material that I have found appears to be a bit blured e.g. client and relying party appear to be the same concept.

The basic concept is you have a client ( a.k.a relying party ) that wants access to some data.  You get the user to authorise access to the data via an authentication server ( a.k.a identity provider ).  The authorisation ultimately comes in the form of an access token.  Once the client has the access token it asks the resource server for the data or to perform an action on the data passing the access token on with the request.  The resource server verifies the access token with the authentication server and if it is sound and grants the appropiate access it will perform the requested action.


### Flows ( a.k.a. Grant types )

* Authorisation code
* Implicit


#### Authorisation code 

1. When the **client** receives a request from a user that it does not have an **access token** for it sends a client redirect response to the browser redirecting them to the **authorisation server**.  There redirect url will include a query string parameter ( redirec_uri ) telling the **authorisation server** where to redirect the browser to once the user has granted access to the resources and a **client id**.  


2. The user grants access to the data, they may or may not have to authenticate them self first this is not defined in the OAuth 2.0 protocol which is why OpenId Connent was added as an extension.  Once the user has completed granting access the **authorisation server** redirects the browser to the redirect_uri specified in step one, the **authentication code** is included as a query string parameter.

3. When the **client** recieves the redirected request it used the **authentication code** to get the **access token** from the **authorisation server**.  The request to the **authorisation server** includes the **authorisation code** the **client id** ( public value ) and the **client secret** ( known only to the client and the authentication server ).  The **client id** and **client secret** are used as a means of the client authenticating itself to the **authorisation server**.  If the client is authenticated the **authorisation server** it will respond with the access token. **Note** - this token should be kept securely as it is the evidence that is presented to the resource servers that they have the right to perform an action


4. When the client want to access resouce data it sends a request to the **resource server** including the **access token**.  The **resouce server** then validated the token via the **authorisation server** and if it is valid and grant the access needed to fulfill the request it will perform that request.

**Note** - This is assuming that the client is going to request resources from a third party RESTful api server, which is what is being refered to as the **resouce server**. 


````
authorisation code
 SplxlOBeZQQYbYS6WxSbIA&state=xyz
````

```
  access token
  {
    "access_token":"03807cb390319329bdf6c777d4dfae9c0d3b3c35",
    "expires_in":3600,
    "token_type":"bearer",
    "scope": "create-task edit-task delete-task"
  }   
```   



#### Implicit

Implicit flow is intended for single page applications and javascript rich clients where it is hard to maintain the privacy required for the client secret.  So instead of an access code being return from step one the **access token** is returned.  The access token is used to in the same was as the authorisation code flow.


### Refresh tokens

A refresh token is a token that a **client** can send to the **authorisation server** when an **access token** expires to get a new one, you get a new **refresh token** as well.


### OpenID Connect

This is an extension added to OAuth 2.0 the ensures that an **identity token** is returned as well as an **access token**.  The **identity token** allows you to check attirbutes such as last logged in time etc.