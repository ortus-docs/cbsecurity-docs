---
description: Create fluent and secure code blocks
---

# Authorization Contexts

There are also times where you need to validate custom conditions and block access to certain areas. This way, you can implement your own custom security logic and leverage **cbSecurity** for blockage. You will accomplish this via the `secureWhen()` method.  If the `context` evaluates to `true` then it will throw a `NotAuthorized` exception for you.

{% hint style="info" %}
Remember, if the `context` is TRUE, then it throws the exception.
{% endhint %}

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

You can also use the `message` argument to send your own message to the exception that's throw.
