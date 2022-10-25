# Verification Methods

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
