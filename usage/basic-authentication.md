---
description: >-
  Basic access authentication is a method for an HTTP user agent (e.g. a web
  browser) to provide a user name and password when making a request.
---

# Basic Authentication

CBSecurity supports the concept of HTTP [basic authentication](https://en.wikipedia.org/wiki/Basic\_access\_authentication) in your ColdBox applications.  Please note that this is a quick and easy way to provide security, but not the safest by any means.  You have been warned!

### What is Basic Authentication?

In the context of an [HTTP](https://en.wikipedia.org/wiki/HTTP) transaction, basic access authentication is a method for an [HTTP user agent](https://en.wikipedia.org/wiki/User\_agent) (e.g. a [web browser](https://en.wikipedia.org/wiki/Web\_browser)) to provide a username and password when making a request. In basic HTTP authentication, a request contains a header field in the form of `Authorization: Basic <credentials>`, where credentials is the [Base64](https://en.wikipedia.org/wiki/Base64) encoding of ID and password joined by a single colon `:`.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Basic Authentication Flow</p></figcaption></figure>

{% hint style="info" %}
HTTP Basic authentication (BA) implementation is the most straightforward technique for enforcing access controls to web resources because it does not require cookies, session identifiers, or login pages; instead, HTTP Basic authentication uses standard fields in the HTTP header.
{% endhint %}

### Configuration

The first step is configuring your application to use [basic authentication](../getting-started/configuration/basic-auth.md) as the [validator](../security-validators/basicauth-validator.md) of choice.  We will configure two things:

* Validator: `BasicAuthValidator@cbsecurity`
* Basic auth settings: Where you configure users, passwords, roles, permissions, and encryption

CBSecurity allows you to use basic authentication with ANY authentication service.

```javascript
cbsecurity : {
    
    basicAuth : {
	users : {
	  "lmajano" : { password : 'test', permissions : "", roles : "admin" }
	}
    },
    
    firewall : {
        // Global Relocation when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthenticationEvent" : "main.index",
        // Default invalid action: override or redirect when an invalid access is detected, default is to redirect
        "defaultAuthenticationAction" : "redirect",
        // Global override event when an invalid access is detected, instead of each rule declaring one.
        "invalidAuthorizationEvent"  : "main.index",
        // Default invalid action: override or redirect when an invalid access is detected, default is to redirect
        "defaultAuthorizationAction" : "redirect",
        // Firewall Validator
        "validator"                   : "BasicAuthValidator@cbsecurity"
    }

}
```

This is the _most_ basic configuration where we register a single user and tell the firewall to use the basic auth validator.  Since the default authentication service is `cbauth` I don't have to register it.  Finally, since CBSecurity detects the `BasicAuthValidator`and no registered user class, it will register the `BasicAuthUserService` as well for you.

{% hint style="success" %}
You can explicitly set the `UserServiceClass` to be `BasicAuthUserService@cbsecurity` if you wanted to.
{% endhint %}



<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Basic Authentication Prompt</p></figcaption></figure>



All I have to do now is create [security rules](untitled-1.md) or [annotations](security-annotations.md), and CBSecurity will leverage the browser's Basic Authentication Prompt when those resources are trying to be accessed.  Once you put in your credentials, it will verify them against the registered users in the `basicAuth` configuration dictionary.

### Logout

Since Basic Authentication ONLY focuses on login, logout is left out of the equation.  In CBSecurity, we have created a special event so you can securely log out users from basic authentication, which you can hit with ANY HTTP verb.

```
/cbsecurity/basicauth/logout
```

This will call the `logout` method of the authentication service and set the following HTTP headers for you so your session can be rotated:

```javascript
event
    .setHTTPHeader( name = "WWW-Authenticate", value = "basic realm='Please enter your credentials'" )
    .setHTTPHeader( name = "Cache-Control", value = "no-cache, must-revalidate, max-age=0" )
    .renderData( data = "<h1>Logout Successful!</h1>", statusCode = 401 );
```

Ultimately, you can close your browser too.

### ColdBox Request Context

ColdBox also supports the concept of basic authentication retrieval since the early version 2 days.  ColdBox can detect, parse and give you a struct of `username` and `password` by leveraging the request context's `getHTTPBasicCredentials()` method.

```javascript
function preHandler( event, action, eventArguments ){
    var authDetails = event.getHTTPBasicCredentials();
    if( !securityService.authenticate( authDetails.username, authDetails.password ) ) {
        event.renderData( type="JSON", data={ message = 'Please check your credentials' }, statusCode=401, statusMessage="You're not authorized to do that");
    }
}
```

{% embed url="https://coldbox.ortusbooks.com/digging-deeper/recipes/building-rest-apis#basic-http-auth" %}
ColdBox HTTP Basic Auth Support
{% endembed %}
