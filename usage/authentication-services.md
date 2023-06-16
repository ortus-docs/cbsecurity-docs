---
description: ColdBox security can work with ANY authentication service provider.
---

# Authentication Services

CBSecurity has been designed to work with **ANY** authentication and user service provider.  CBSecurity is in charge of intercepting requests and delegating access verification to _Security Validators_, leveraging _Authentication_ and _User Services_ to allow access to a resource or block the request ultimately. &#x20;

<figure><img src="../.gitbook/assets/CBSecurity - Security Flow.png" alt=""><figcaption><p>The CBSecurity Security Flow</p></figcaption></figure>

We have created an [interface](authentication-services.md#authentication-service-interface) that must be implemented by any service that is going to be used with CBSecurity: `cbsecurity.interfaces.IAuthService`.  Then you would configure this service in the [Configuration](../getting-started/configuration/authentication.md) File alongside the validator you would like to use (cbauth, JWT, basic auth, etc.)

```javascript
/**
 * --------------------------------------------------------------------------
 * Authentication Services
 * --------------------------------------------------------------------------
 * Here you will configure which service is in charge of providing authentication for your application.
 * By default we leverage the cbauth module which expects you to connect it to a database via your own User Service.
 *
 * Available authentication providers:
 * - cbauth : Leverages your own UserService that determines authentication and user retrieval
 * - basicAuth : Leverages basic authentication and basic in-memory user registration in our configuration
 * - custom : Any other service that adheres to our IAuthService interface
 */
authentication : {
	// The WireBox ID of the authentication service to use which must adhere to the cbsecurity.interfaces.IAuthService interface.
	"provider"        : "authenticationService@cbauth",
	// WireBox ID of the user service to use when leveraging user authentication, we default this to whatever is set
	// by cbauth or basic authentication. (Optional)
	"userService"     : cbauth.userServiceclass,
	// The name of the variable to use to store an authenticated user in prc scope on all incoming authenticated requests
	"prcUserVariable" : "oCurrentUser"
},

firewall : {
        "validator" : "CBAuthValidator@cbsecurity"
}
```

By default, CBSecurity ships with a very simple yet powerful authentication service and validator called [cbauth](https://forgebox.io/view/cbauth).  This module gives you the ability to login, logout, verify and tracker users across requests using session and request storages.  All you have to do is provide a User Service class that will connect to your storage of choice in order to operate.  Here is a typical `cbauth` configuration that will exist alongside the `cbsecurity` module settings:

```javascript
cbauth = {
    // This is the path to your user object that contains the credential validation methods
    userServiceClass = "MyUserService"
},
```
This user service must also adhere to our User Service interface: `cbsecurity.interfaces.IUserService` and the user objects it must produce also will need to adhere to our user interface: `cbsecurity.interface.IAuthUser`.

If you are using `cbauth`, please keep in mind that it stores only the user id in session. All other `AuthUser` properites are transient as they are part of the request scope.

## IAuthService

This interface has been provided by convenience, and it is not mandatory at runtime since cbauth implements it: (`cbsecurity.interfaces.IAuthService`)

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

You can find the information for **cbauth** in its book:

{% embed url="https://cbauth.ortusbooks.com" %}

## IAuthUser

As you can see from above, the authentication services all expect a `User` object to model your user in the system. So your `User` object must also adhere to the following methods modeled by the `cbsecurity.interfaces.IAuthUser` interface. This will allow the validators and JWT services to get the appropriate data.

{% code title="cbsecurity.interfaces.IAuthUser.cfc" %}
```javascript
/**
 * Copyright since 2016 by Ortus Solutions, Corp
 * www.ortussolutions.com
 * ---
 * If you use a user with a user service or authentication service, it must implement this interface
 */
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
     * Verify if the user has one or more of the passed in roles
     *
     * @role One or a list of roles to check for access
     *
     */
    boolean function hasRole( required role );

}
```
{% endcode %}

## IUserService

If you are using **cbauth** or any of our JWT features, then we will also require you to register a user service class that can provide us with the correct data to encapsulate security using the `userService` setting. We have provided this interface for your usage:

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

## Simple Example

Ok, now that we have discovered the basics of CBSecurity, why don't we build a simple example using a database-driven approach to security with `cbauth`.  Please note that we also have a [Basic Authentication](basic-authentication.md) approach as well.

### Configuration

We will use the defaults CBSecurity ships with to protect our new admin console with `cbauth`.

```javascript
moduleSettings : {
    cbauth : {
        // Our service class
	userServiceClass : "UserService"
    },
	
    cbsecurity : {
        // The global invalid authentication event or URI or URL to go if an invalid authentication occurs
	"invalidAuthenticationEvent"  : "security.login",
	// Default Auhtentication Action: override or redirect when a user has not logged in
	"defaultAuthenticationAction" : "redirect",
	// The global invalid authorization event or URI or URL to go if an invalid authorization occurs
	"invalidAuthorizationEvent"   : "security.notAuthorized",
	// Default Authorization Action: override or redirect when a user does not have enough permissions to access something
	"defaultAuthorizationAction"  : "redirect",
	// Firewall database event logs.
	"logs" : {
		"enabled"    : true,
		"table"      : "cbsecurity_logs"
	},
	rules : [
	    {
		    secureList : "^admin"
	    }
	]
    }
};
```

As you can see, we don't have to specify an authentication provider or validator; it's already defaulted to `cbauth`.  I only have to specify the user service that will provide the `User` object and user information from my database.

### User

Ok, before I go into building my user service, I would have to create a `User` object that the service would return so `cbauth` can use it. However, `CBSecurity` already ships with a basic authentication user object I can use: `cbsecurity.models.basicauth.BasicAuthUser`.&#x20;

I will model my database table after it and create the following columns:

* `id`
* `firstName`
* `lastName`
* `username`
* `password`
* `permissions`
* `roles`

```javascript
/**
 * This is a basic user object that can be used with cbsecurity.
 * It implements the following interfaces
 * - cbsecurity.interfaces.jwt.IJwtSubject
 * - cbsecurity.interfaces.IAuthUser
 */
component accessors="true" {

	property name="id";
	property name="firstName";
	property name="lastName";
	property name="username";
	property name="password";
	property name="permissions";
	property name="roles";

	function init(){
		variables.id        = "";
		variables.firstName = "";
		variables.lastName  = "";
		variables.username  = "";
		variables.password  = "";

		variables.permissions = [];
		variables.roles       = [];

		return this;
	}

	function setRoles( roles ){
		if ( isSimpleValue( arguments.roles ) ) {
			arguments.roles = listToArray( arguments.roles );
		}
		variables.roles = arguments.roles;
		return this;
	}

	function setPermissions( permissions ){
		if ( isSimpleValue( arguments.permissions ) ) {
			arguments.permissions = listToArray( arguments.permissions );
		}
		variables.permissions = arguments.permissions;
		return this;
	}

	/**
	 * Verify if this is a valid user or not
	 */
	boolean function isLoaded(){
		return ( !isNull( variables.id ) && len( variables.id ) );
	}

	/**
	 * A struct of custom claims to add to the JWT token
	 */
	struct function getJWTCustomClaims( required struct payload ){
		return { "role" : variables.roles.toList() };
	}

	/**
	 * This function returns an array of all the scopes that should be attached to the JWT token that will be used for authorization.
	 */
	array function getJWTScopes(){
		return variables.permissions;
	}

	/**
	 * Verify if the user has one or more of the passed in permissions
	 *
	 * @permission One or a list of permissions to check for access
	 */
	boolean function hasPermission( required permission ){
		if ( isSimpleValue( arguments.permission ) ) {
			arguments.permission = listToArray( arguments.permission );
		}

		return arguments.permission
			.filter( function( item ){
				return ( variables.permissions.findNoCase( item ) );
			} )
			.len();
	}

	/**
	 * Verify if the user has one or more of the passed in roles
	 *
	 * @role One or a list of roles to check for access
	 */
	boolean function hasRole( required role ){
		if ( isSimpleValue( arguments.role ) ) {
			arguments.role = listToArray( arguments.role );
		}

		return arguments.role
			.filter( function( item ){
				return ( variables.roles.findNoCase( item ) );
			} )
			.len();
	}

}

```

### User Service

Ok, now let's build our basic service that will leverage the DB and simple password hashing.  Remember, this object must implement `cbsecurity.interfaces.IUserService` :

```javascript
component accessors="true" singleton {

	/*********************************************************************************************/
	/** DI **/
	/*********************************************************************************************/

	property name="populator" inject="wirebox:populator";
	property name="wirebox"   inject="wirebox";

	/*********************************************************************************************/
	/** Static Settings **/
	/*********************************************************************************************/

	static {
		hashAlgorithm = "SHA-512",
		hashIterations = 5
	}

	/**
	 * Constructor
	 */
	function init(){
		return this;
	}

	/**
	 * Hash the incoming target according to our hashing algorithm and settings
	 * @target The string target to hash
	 */
	private string function hashSecurely( required string target ){
		return hash( arguments.target, static.hashAlgorithm, "UTF-8", static.hashIterations );
	}

	/**
	 * New User Dispenser
	 */
	BasicAuthUser function new() provider="BasicAuthUser@cbsecurity"{
	}

	/**
	 * Get a new user by id
	 *
	 * @id The id to get the user with
	 *
	 * @return The located user or a new un-loaded user object
	 */
	BasicAuthUser function retrieveUserById( required id ){
		return queryExecute( 
			"select * from users where id = :id",
			{ id : arguments.id },
			{ returnType : "array" }
		).reduce( ( result, data ) =>{
			return populator.populateFromStruct( new(), arguments.data );
		}, new() );
	}

	/**
	 * Get a user by username
	 *
	 * @username The username to get the user with
	 *
	 * @return The valid user object representing the username or an empty user object
	 */
	BasicAuthUser function retrieveUserByUsername( required username ){
		return queryExecute( 
			"select * from users where username = :username",
			{ username : arguments.username },
			{ returnType : "array" }
		).reduce( ( result, data ) =>{
			return populator.populateFromStruct( new(), arguments.data );
		}, new() );
	}

	/**
	 * Verify if the incoming username and password are valid credentials in this user storage
	 *
	 * @username The username to test
	 * @password The password to test
	 *
	 * @return true if valid, else false
	 */
	boolean function isValidCredentials( required username, required password ){
		var oUser = retrieveUserByUsername( arguments.username );
		if ( !oUser.isLoaded() ) {
			return false;
		}

		return hashSecurely( arguments.password ) eq oUser.getPassword();
	}

}

```

### Testing It Out

At this point, I have satisfied all the requirements for CBSecurity to work:

1. Create a user service that knows how to get users by id, username, and credentials
2. Created my database table and connection

Now I have to build out:

1. Login screen
2. Login processor
3. Logout processor

#### Login Handler and Screen

```javascript
component{
    function login( event, rc, prc ){
        event.setView( "security.login" );
    }
}
```

```html
<cfoutput>
<h1>Security Login</h1>

<cfif flash.exists( "message" )>
  <div style="border: 1px solid gray; background-color: ##f29595; margin: 20px 0px; padding: 10px">
	#flash.get( "message" )#
  </div>
</cfif>

#html.startForm( action="security.doLogin" )#

  #html.textField( name="username", placeholder="username" )#
  <br>
  #html.passwordField( name="password", placeholder="password" )#
  <br>
  #html.submitButton( name="Submit" )#

#html.endForm()#
</cfoutput>
```

#### Login-Logout Processor

```javascript
component{
    function doLogin( event, rc, prc ){
        try {
	    var oUser = cbsecure().authenticate( rc.username ?: "", rc.password ?: "" );
	    return "You are logged in!";
	} catch ( "InvalidCredentials" e ) {
	    flash.put( "message", "Invalid credentials, try again!" );
	    relocate( "security/login" );
	}
    }
    
    function doLogout( event, rc, prc ){
	cbsecure().logout();
	flash.put( "message", "Bye bye!" );
	relocate( "security/login" );
    }
}
```

{% hint style="info" %}
You can also use the `guest()` method to verify if the user is NOT logged in.
{% endhint %}
