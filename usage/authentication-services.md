# Authentication Services

You can register ANY authentication provider with **cbsecurity** by using the `authenticationService` setting. The value must be a valid WireBox Id and the object must adhere to the following interface. The authentication services can be used in conjunction with our JWT services and more features coming in the future.

{% hint style="info" %}
Please note that **cbauth** already implements this interface and is included with **cbsecurity** as a dependency.
{% endhint %}

## Authentication Service Interface

This interface has been provided by convenience and it is not mandatory at runtime \(`cbsecurity.interfaces.IAuthService`\)

{% code-tabs %}
{% code-tabs-item title="cbsecurity.interfaces.IAuthService.cfc" %}
```javascript
interface{

    /**
     * Get the authenticated user
     *
     * @return User that implements IAuthUser
     * @throws NoUserLoggedIn
     */
    any function getUser();

    /**
     * Verifies if a user is logged in
     */
    boolean function isLoggedIn();

    /**
     * Attemps to log in a user
     *
     * @username The username to log in with
     * @password The password to log in with
     *
     * @throws InvalidCredentials
     */
    boolean function authenticate( required username, required password );

    /**
     * Logs a user into the system
     *
     * @user The user object that implements IAuthUser
     */
    function login( required user );

    /**
     * Logs out the currently logged in user from the system
     */
    function logout();


}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can find the information for cbauth at their website: [https://github.com/elpete/cbauth/](https://github.com/elpete/cbauth/)

## User Interface

As you can see from above, the authentication services all expect a user object to model your user in the system.  So your user object must also adhere to the following methods modeled by the `cbsecurity.interfaces.IAuthUser` interface.  This will allow the validators and jwt services to get the appropriate data it needs.

{% code-tabs %}
{% code-tabs-item title="cbsecurity.interfaces.IAuthUser.cfc" %}
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

    /**
     * Shortcut to verify it the user is logged in or not.
     */
    boolean function isLoggedIn();

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## User Services

If you will be using cbauth or any of our jwt features, then we will also require you register a user service class that can provide us with the right data to encapsulate security using the `userService` setting.  We have provided this interface for your usage:

{% code-tabs %}
{% code-tabs-item title="cbsecurity.interfaces.IUserService.cfc" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

