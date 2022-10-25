---
description: >-
  This object is used to provide you with human, fluent and explicit security
  authorizations and contexts.
---

# cbSecurity Model

## Explicit Authorizations

The `cbSecurity` model is a specialized service that will allow you to do explicit authorizations in any layer of your ColdBox application.

There will be times where you will need authorization checks outside of the incoming request rules or the handler annotations. This can be from within interceptors, models, layouts or even views. For this, we have provided the `cbSecurity` model so you can do explicit authorization checks anywhere you like.

## `cbSecurity` Model

You can inject our model or you can use our handy `cbsecure()` mixin (handlers/layouts/views) and then call the appropriate security functions:

```javascript
// Mixin: Handlers/Layouts/Views
cbsecure()

// Injection
property name="cbSecurity" inject="@cbSecurity"
```

{% hint style="danger" %}
All security methods will call the application's configured Authentication Service to retrieve the currently logged in user. If the user is not logged in an immediate `NoUserLoggedIn` exception will be thrown by all methods.
{% endhint %}

You can now discover our sections for securing using `cbSecurity`

* [Secure() blocking methods](secure-blocking-methods.md)
* [Verification Methods](verification-methods.md)
* [Authorization Contexts](authorization-contexts.md)
* [Securing Views](securing-views.md)

## `cbSecurity` Method Summary

### **Blocking Methods**

When certain permission context is met, if not throws `NotAuthorized`

* `secure( permissions, [message] )`
* `secureAll( permissions, [message] )`
* `secureNone( permissions, [message] )`
* `secureWhen( context, [message] )`
* `guard() alias to secure()`

### **Action Context Methods**

When certain permission context is met, execute the success function/closure, else if a `fail` closure is defined, execute that instead.

* `when( permissions, success, fail )`
* `whenAll( permissions, success, fail )`
* `whenNone( permissions, success, fail )`

### **Verification Methods**

Verify permissions or user equality

* `has( permissions ):boolean`
* `all( permissions ):boolean`
* `none( permissions ):boolean`
* `sameUser( user ):boolean`

### **Request Context Methods**

* `secureView( permissions, successView, failView )`
