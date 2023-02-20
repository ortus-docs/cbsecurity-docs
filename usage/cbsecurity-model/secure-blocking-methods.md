---
description: Get outta here!
---

# Blocking Methods

### The `secure()` Methods

Now that you have access to the model, you can use the following method to verify explicit permissions and authorize access. This method will **throw an exception** if the user does not validate the incoming permissions context (`NotAuthorized`).

```javascript
// Verify the currently logged in user has at least one of those permissions, 
// else throw a NotAuthorized exception
cbSecurity.secure( permissions, [message] );
cbsecure().secure( permissions, [message] );
```

* The `permission` can be an array, string or list of the permissions to validate. The user must have at least one of the permissions specified.
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

* The `permissions` is a permission array or list that will be Or'ed&#x20;
* The `success` is a closure/lambda or UDF that will execute if the permissions validate. &#x20;
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
