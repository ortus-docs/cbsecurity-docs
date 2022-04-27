---
description: ColdBox security can work with ANY authentication service provider.
---

# Authentication Services

You can register ANY authentication provider with **cbsecurity** by using the `authenticationService` setting. The value must be a valid WireBox Id and the object must adhere to the following [interface](authentication-services.md#authentication-service-interface). The authentication services can be used in conjunction with our JWT services and more features coming in the future.

```javascript
// CB Security
cbSecurity : {
    
    // The WireBox ID of the authentication service to use in cbSecurity which must adhere to the cbsecurity.interfaces.IAuthService interface.
    "authenticationService" : "authenticationService@cbauth"
    
}
```

{% hint style="info" %}
Please note that **cbauth** already implements this interface and is included with **cbsecurity** as a dependency.
{% endhint %}

## Authentication Service Interface

This interface has been provided by convenience and it is not mandatory at runtime (`cbsecurity.interfaces.IAuthService`)

{% code title="cbsecurity.interfaces.IAuthService.cfc" %}
```javascript
/**
 * Copyright since 2016 by Ortus Solutions, Corp
 * www.ortussolutions.com
 * ---
 * If you register an authentication service with cbsecurity it must adhere to this interface
 */
interface{

    /**
     * Get the authenticated user
     *
	   * @throws NoUserLoggedIn : If the user is not logged in
	   *
     * @return User that implements IAuthUser
     */
    any function getUser();

    /**
     * Verifies if a user is logged in
     */
    boolean function isLoggedIn();

    /**
     * Try to authenticate a user into the system. If the authentication fails an exception is thrown, else the logged in user object is returned
     *
     * @username The username to log in with
     * @password The password to log in with
     *
     * @throws InvalidCredentials
	   *
	   * @return User : The logged in user object
     */
    any function authenticate( required username, required password );

    /**
  	 * Login a user into our persistent scopes
  	 *
  	 * @user The user object to log in
  	 *
  	 * @return The same user object so you can do functional goodness
	   */
    function login( required user );

    /**
     * Logs out the currently logged in user from the system
     */
    function logout();


}
```
{% endcode %}

You can find the information for **cbauth** in its own book:

{% embed url="https://cbauth.ortusbooks.com" %}

{% hint style="warning" %}
If you are using cbauth as your `authenticationService` (the default), you also need to [configure cbauth.](https://cbauth.ortusbooks.com/installation-and-usage)
{% endhint %}

## User Interface

As you can see from above, the authentication services all expect a `User` object to model your user in the system. So your `User` object must also adhere to the following methods modeled by the `cbsecurity.interfaces.IAuthUser` interface. This will allow the validators and JWT services to get the appropriate data it needs.

{% code title="cbsecurity.interfaces.IAuthUser.cfc" %}
```javascript
interface{

    /**
     * Return the unique identifier for the user
     */
    function getId();

    /**
     * Verify if the user has one or more of the passed in permissions
     *
     * @permission One or a list of permissions to check for access
     *
     */
    boolean function hasPermission( required permission );

}
```
{% endcode %}

## User Services

If you will be using **cbauth** or any of our JWT features, then we will also require you register a user service class that can provide us with the right data to encapsulate security using the `userService` setting. We have provided this interface for your usage:

{% code title="cbsecurity.interfaces.IUserService.cfc" %}
```javascript
interface{

    /**
     * Verify if the incoming username/password are valid credentials.
     *
     * @username The username
     * @password The password
     */
    boolean function isValidCredentials( required username, required password );

    /**
     * Retrieve a user by username
     *
     * @return User that implements JWTSubject and/or IAuthUser
     */
    function retrieveUserByUsername( required username );

    /**
     * Retrieve a user by unique identifier
     *
     * @id The unique identifier
     *
     * @return User that implements JWTSubject and/or IAuthUser
     */
    function retrieveUserById( required id );
}
```
{% endcode %}

{% hint style="warning" %}
If using `cbauth`, you also have to specify the `UserServiceClass` key in the **cbauth** module settings.
{% endhint %}

{% hint style="info" %}
Remember that the User Service setting is only required if you will be using JWT token security
{% endhint %}
