---
description: 2020-MAR-30
---

# What's New With 2.3.0

This release focuses on bringing a strong focus on protecting every development layer of a ColdBox application.  It introduces the `cbSecurity` model that can be used by any layer and provides you with a great functional API to secure your code.

## Explicit Authorizations

There will be times where you will need authorization checks outside of the incoming request rules or the handler annotations. This can be from within interceptors, models, layouts or even views. For this, we have provided the `cbSecurity` model so you can do explicit authorization checks anywhere you like.

## `cbSecurity` Model

You can inject our model or you can use our handy `cbsecure()` mixin \(interceptors/handlers/layouts/views\) and then call the appropriate security functions:

```javascript
// Mixin: Handlers/Interceptors/Layouts/Views
cbsecure()

// Injection
property name="cbSecurity" inject="@cbSecurity"
```

{% hint style="danger" %}
All security methods will call the application's configured Authentication Service to retrieve the currently logged in user. If the user is not logged in an immediate `NoUserLoggedIn` exception will be thrown by all methods.
{% endhint %}

### The `secure()` Methods

Now that you have access to the model, you can use the following method to verify explicit permissions and authorize access. This method will **throw an exception** if the user does not validate the incoming permissions context \(`NotAuthorized`\).

```javascript
// Verify the currently logged in user has those permission, 
// else throw a NotAuthorized exception
cbSecurity.secure( permissions, [message] );
cbsecure().secure( permissions, [message] );
```

* The `permission` can be an array, string or list of the permissions to validate.
* The `message` is a custom error message to be used in the `message` string of the exception thrown.

You also have two more authorization methods that will verify certain permission conditions for you:

```javascript
// Authorize that the user has ALL of the incoming permissions
cbSecurity.secureAll( permissions, [message] );
// Authorize that the user has NONE of the incoming permissions
cbSecurity.secureNone( permissions, [message] );
```

### Conditional Authorizations Using `when()`

There are also cases where you want to execute a piece of code by determining if the user has access to do so. For example, only a `USER_ADMIN` can change people's roles or you want to filter some data for certain users. For this, we have created the `when()` method with the following signature:

```javascript
when( permissions, success, fail )
```

* The `permissions` is a permission array or list that will be Or'ed 
* The `success` is a closure/lambda or UDF that will execute if the permissions validate.  
* The `fail` is a closure/lambda or UDF that will execute if the permissions DID not validate, much like an else statement

Both closures/functions takes in a `user` which is the currently authenticated user, the called in `permissions` and can return anything.

```javascript
// Lambda approach
( user, permissions ) => { 
    // your code here
};
// UDF/Closure
function( user, permissions ){ 
    // your code here
}
```

You can also chain the `when()` calls if needed, to create beautiful security contexts. So if we go back to our admin examples, we can do something like this:

```javascript
var oAuthor = authorService.getOrFail( rc.authorId );
prc.data = userService.getData();

// Run Security Contexts
cbSecure()
    // Only user admin can change to the incoming role
    .when( "USER_ADMIN", ( user ) => oAuthor.setRole( roleService.get( rc.roleID ) ) )
    // The system admin can set a super admin
    .when( "SYSTEM_ADMIN", ( user ) => oAuthor.setRole( roleService.getSystemAdmin() ) )
    // Filter the data to be shown to the user
    .when( "USER_READ_ONLY", ( user ) => prc.data.filter( ( i ) => !i.isClassified ) )

// Calling with a fail closure
cbSecurity.when(
    "USER_ADMIN",
    ( user ) => user.setRole( "admin" ), //success
    ( user ) => relocate( "Invaliduser" ) //fail
);
```

We have also added the following `whenX()` methods to serve your needs when evaluating the permissions:

```javascript
// When all permissions must exist in the user
whenAll( permissoins, success, fail)
// When none of the permissions exist in the user
whenNone( permissions, success, fail )
```

### Verification Methods

If you just want to validate if a user has certain permissions or maybe no permissions at all or if a passed user is the same as the logged in user, then you can use the following boolean methods that only do verification.

{% hint style="success" %}
Please note that you could potentially do these type of methods by leveraging the currently logged in user and it's `hasPermission()` method. However, these methods provide abstraction and can easily be mocked!
{% endhint %}

```javascript
// Checks the user has one or at least one permission if the 
// permission is a list or array
boolean cbSecurity.has( permission );
// The user must have ALL the permissions
boolean cbSecurity.all( permission );
// The user must NOT have any of the permissions
boolean cbSecurity.none( permission );
// Verify if the passed in user is the same as the logged in user
boolean cbSecurity.sameUser( user );
```

These are great to have a unified and abstracted way to verifying permissions or if the passed user is the same as the logged in user. Here are some examples:

**View Layer**

```markup
<cfif cbsecure().has( "USER_ADMIN" )>
    This is only visible to user admins!
</cfif>

<cfif cbsecure().has( "SYSTEM_ADMIN" )>
    <a href="/user/impersonate/#prc.user.getId()#">Impersonate User</a>
</cfif>

<cfif cbsecure().sameUser( prc.user )>
    <i class="fa fa-star">This is You!</i>
</cfif>
```

Other Layers:

```javascript
if( cbSecurity.has( "PERM" ) ){
    auditUser();
}

if( cbSecurity.sameUser( prc.incomingUser ) ){
    // you can change your gravatar
}
```

{% hint style="info" %}
Please note that we do user equality by calling the `getId()` method of the authenticated user and the incoming user. This is part of our `IAuthUser` interface requirements.
{% endhint %}

### Authorization Contexts

There are also times where you need to validate custom conditions and block access to certain areas. This way, you can implement your own custom security logic and leverage **cbSecurity** for blockage. You will accomplish this via the `secureWhen()` method:

```javascript
secureWhen( context, [errorMessage] )
```

The `context` can be a closure/lambda/udf or a boolean evaluation:

```javascript
// Using as a closure/lambda
cbSecurity.secureWhen( ( user ) => !user.isConfirmed() )
cbSecurity.secureWhen( ( user ) => !oEntry.canPublish( user ) )

// Using a boolean evaluation
cbSecurity.secureWhen( cbSecurity.none( "AUTHOR_ADMIN" ) && !cbSecurity.sameUser( oAuthor )  )
cbSecurity.whenNone( "AUTHOR_ADMIN", ( user ) => relocate() );
```

The closure/udf will receive the currently authenticated user as the first argument.

```javascript
( user ) => {}
function( user );
```

### Securing Views

You can also use our handy `event.secureView()` method in the request context to pivot between views according to user permissions.

{% hint style="info" %}
cbSecurity injects the `secureView()` method into the request context via the `preProcess` interception point.
{% endhint %}

```javascript
event.secureView( permissions, successView, failView )
```

This will allow you to set the `successView` if the user has the permissions or the `failView` if they don't.

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

