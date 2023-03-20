# JWT Services

CBSecurity also provides you with a JWT (Json Web Tokens) authentication and authorization system.

JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the **HMAC** algorithm) or a public/private key pair using **RSA** or **ECDSA**.

![](../.gitbook/assets/68747470733a2f2f63646e2e61757468302e636f6d2f636f6e74656e742f6a77742f6a77742d6469616772616d2e706e67.png)

Signed tokens can verify the _integrity_ of the **claims** contained within it, while encrypted tokens _hide_ those claims from other parties. When tokens are signed using public/private key pairs, the signature also certifies that only the party holding the private key is the one that signed it.

![](../.gitbook/assets/Why-Cant-I-Just-Send-JWTs-Without-OAuth-JWT.png)

You can find much more information about JWT at [jwt.io](https://jwt.io/introduction/).

{% embed url="https://jwt.io/introduction/" %}

## When should you use JSON Web Tokens?

JSON Web Tokens have become the standard for authenticating and authorizing API requests. They can be used on their own or with an oauth/single sign-on server as well.

* **Authorization**: This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.
* **Information Exchange**: JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

The ColdBox Security module will assist you with all the generation, decoding, encoding and security aspects of JWT. All you need to do is, configure it, create a few standard files and off you go.

## Tokens

The tokens created by the JWT services will have the mandatory headers, but also will have a standardizes payload structure. This payload structure can also be customized as you see fit.

![](<../.gitbook/assets/Screen Shot 2019-09-24 at 1.42.21 PM.png>)

A JSON Web Token encodes a series of claims in a JSON object. Some of these claims have specific meanings, while others are left to be interpreted by the users. You can consider claims to be the keys of the payload structure, and it can contain pretty much anything you like.

### Base Claims

Here are the base claims that the ColdBox Security JWT token creates for you automatically:

* Issuer (`iss`) - The issuer of the token (defaults to the application's base URL)
* Issued At (`iat`) - When the token was issued (Unix timestamp)
* Subject (`sub`) - This holds the identifier for the token (defaults to user id)
* Expiration time (`exp`) - The token expiry date (Unix timestamp)
* Unique ID (`jti`) - A unique identifier for the token (`md5` of the `sub` and `iat` claims)
* Scopes (`scope)` - A space-delimited string of scopes attached to the token
* Refresh Token (`cbsecurity_refresh` ) - If you use refresh tokens, this custom claim will be added to the payload.

{% code title="mytoken.json" %}
```javascript
{
  "iat": 1569340662,
  "scope": "",
  "iss": "http://127.0.0.1:56596/",
  "sub": 123,
  "exp": 1569344262,
  "jti": "12954F907C0535ABE97F761829C6BD11"
}
```
{% endcode %}

{% code title="myRefreshToken.json" %}
```javascript
{
  "iat": 1569340662,
  "scope": "",
  "iss": "http://127.0.0.1:56596/",
  "sub": 2222,
  "exp": 1569344262,
  "jti": "234234CDDEEDD",
  "cbsecurity_refresh" : true
}
```
{% endcode %}

You can add much more to this payload via the JWT service methods or the User that models the token.

## Our JwtService

The service can be found here `cbsecurity.models.jwt.JWTService` and can be retrieved by either injecting the service (`JwtService@cbsecurity`) or using our helper method (`jwtAuth()`).

```javascript
// Injection
property name="jwtService" inject="JwtService@cbsecurity";

// Helper Method in any handler/layout/interceptors/views
jwtAuth()
```

To begin exploring the JWT capabilities, let's explore how to configure it first.

## Configuration

You can check out our [JWT Configuration section](../getting-started/configuration/jwt.md) for a more in-depth tutorial.  Here are the basic settings you would put in the `cbsecurity` configuration:

```javascript
cbsecurity : {
    // The WireBox ID of the authentication service to use in cbSecurity which must adhere to the cbsecurity.interfaces.IAuthService interface.
    authenticationService  : "authenticationService@cbauth",
    // WireBox ID of the user service to use
    userService             : "",
    // The name of the variable to use to store an authenticated user in prc scope if using a validator that supports it.
    prcUserVariable         : "oCurrentUser",
    // JWT Settings
    jwt                     : {
        // The issuer authority for the tokens, placed in the `iss` claim
        issuer                          : "",
        // The jwt secret encoding key, defaults to getSystemEnv( "JWT_SECRET", "" )
        // This key is only effective within the `config/Coldbox.cfc`. Specifying within a module does nothing.
        secretKey               : getSystemSetting( "JWT_SECRET", "" ),
        // by default it uses the authorization bearer header, but you can also pass a custom one as well.
        customAuthHeader        : "x-auth-token",
        // The expiration in minutes for the jwt tokens
        expiration              : 60, 
        // If true, enables refresh tokens, token creation methods will return a struct instead
        // of just the access token. e.g. { access_token: "", refresh_token : "" }
        enableRefreshTokens        : false,
        // The default expiration for refresh tokens, defaults to 30 days
        refreshExpiration          : 10080,
        // The Custom header to inspect for refresh tokens
        customRefreshHeader        : "x-refresh-token",
        // If enabled, the JWT validator will inspect the request for refresh tokens and expired access tokens
        // It will then automatically refresh them for you and return them back as
        // response headers in the same request according to the customRefreshHeader and customAuthHeader
        enableAutoRefreshValidator : false,
        // Enable the POST > /cbsecurity/refreshtoken API endpoint
        enableRefreshEndpoint      : true,
        // encryption algorithm to use, valid algorithms are: HS256, HS384, and HS512
        algorithm               : "HS512",
        // Which claims neds to be present on the jwt token or `TokenInvalidException` upon verification and decoding
        requiredClaims          : [] ,
        // The token storage settings
        tokenStorage            : {
            // enable or not, default is true
            "enabled"       : true
            // A cache key prefix to use when storing the tokens
            "keyPrefix"     : "cbjwt_", 
            // The driver to use: db, cachebox or a WireBox ID
            "driver"        : "cachebox",
            // Driver specific properties
            "properties"    : {
                cacheName : "default"
            }
        }
    }
}
```



## JWT Subject Interface

The next step is ensuring that our JWT services can handle the construction of the JWT tokens as per YOUR requirements. So your `User` object must implement our `JWTSubject` interface with the following functions:

{% code title="cbsecurity.interfaces.jwt.IJwtSubject.cfc" %}
```javascript
/**
 * Copyright since 2016 by Ortus Solutions, Corp
 * www.ortussolutions.com
 * ---
 * If you use the jwt services, then your jwt subject user must implement this interface
 */
interface{

    /**
     * A struct of custom claims to add to the JWT token when creating it
     *
     * @payload The actual payload structure that was used in the request
     *
     * @return A structure of custom claims
     */
    struct function getJwtCustomClaims( required struct payload );

    /**
     * This function returns an array of all the scopes that should be attached to the JWT token that will be used for authorization.
     */
    array function getJwtScopes();

}

```
{% endcode %}

Basically, it's two functions:

* `getJwtCustomClaims( payload )` - This is a struct of custom claims to incorporate into the token payload at construction time. This can be ANYTHING you like.
* `getJwtScopes()` - We will also call this at construction time in order to incorporate the right permission scopes into the token according to your user. This must be an array of scopes/permissions.

Since also the authentication services will be used with JWT, your user object might end up looking like this:

{% code title="models/User.cfc" %}
```javascript
component accessors="true" {

    property name="auth" inject="authenticationService@cbauth";

    property name="id";
    property name="firstName";
    property name="lastName";
    property name="username";
    property name="password";

    function init(){
        variables.id        = "";
        variables.firstName = "";
        variables.lastName  = "";
        variables.username  = "";
        variables.password  = "";

        variables.permissions = [ "write", "read" ];

        return this;
    }

    boolean function isLoaded(){
        return ( !isNull( variables.id ) && len( variables.id ) );
    }

    /**
     * A struct of custom claims to add to the JWT token
     */
    struct function getJWTCustomClaims(){
        return { "role" : "admin" };
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
     *
     */
    boolean function hasPermission( required permission ){
        if ( isSimpleValue( arguments.permission ) ) {
            arguments.permission = listToArray( arguments.permission );
        }

        return arguments.permission
            .filter( function(item){
                return ( variables.permissions.ListFindNoCase( item ) );
            } )
            .len();
    }

}
```
{% endcode %}

## Authentication and User Services

The JWT validators must talk to the authentication and user services. Please refer to the [Authentication Services](../usage/authentication-services.md) page to configure and create them.

## JWT Methods

Ok, now we can focus on all the wonderful methods the JWT service offers:

### Token Creation Methods

* `attempt( username, password, [ customClaims:struct ] ):token` - Attempt to authenticate a user with the authentication service and if successful, return the token using the identifier and custom claims. Exception if invalid authentication
* `fromUser( user, [ customClaims:struct ] ):token` - Generate a token according to the passed user object and custom claims.

### Raw JWT Methods

* `encode( struct payload ):token` - Generate a raw JWT token from a native payload struct.
* `verify( required token ):boolean` - Verify a token string or throws an exception
* `decode( required token ):struct` - Decode and retrieve the passed-in token to CFML struct

### Parsing and Helper Methods

* `parseToken( token, storeInContext, authenticate ):struct` - Get the decoded token using the headers strategy and store it in the `prc.jwt_token` and the decoded data as `prc.jwt_payload` if it verifies correctly. Throws: `TokenExpiredException` if the token is expired, `TokenInvalidException` if the token doesn't verify decoding, `TokenNotFoundException` if not found
* `getToken():string` - Get the stored token from `prc.jwt_token`, if it doesn't exist, it tries to parse it via `parseToken()`, if not token is set, this will be an empty string.
* `getPayload():struct` - Get the stored token from `prc.jwt_payload`, if it doesn't exist, it tries to parse it via `parseToken()`, if not token is set, this will be an empty struct.
* `setToken( token ):JWTService` - Store the token in `prc.jwt_token`, and store the decoded version in `prc.jwt_payload`

### Authentication Helpers

* `authenticate( [payload] ):User` - Authenticates a passed or detected token payload and return the user it represents
* `getuser()` - Get the authenticated user according to the access token detected
* `logout()` - Logout a user and invalidate their token

### Storage Methods

* `invalidateAll( async:false )` - Invalidate all access and refresh tokens in permanent storage
* `invalidate( token )` - Invalidates the incoming token by removing it from the permanent storage.
* `isTokenInStorage( token )` - Checks if the passed token exists in permanent storage.
* `getTokenStorage( force:false )` - Get the current token storage implementation. You can also force-create it again if needed.

### Refresh Methods

* `attempt( username, password, [ customClaims:struct ] ):struct` - Attempt to authenticate a user with the authentication service and if successful, return a struct containing an access and refresh token.
* `fromUser( user, [ customClaims:struct ] ):struct` - Generate a struct of refresh and access tokens according to the passed user object and custom claims.

```javascript
{
    "access_token"  : "AYjcyMzY3ZDhiNmJk",
    "refresh_token" : "RjY2NjM5NzA2OWJj"
}
```

## Putting it Together

That's it, we are ready to put it all together. Now cbsecurity knows about your authentication/user services, can talk to your user to create tokens and can guard the incoming requests via the JWT Validator. Here is a sample controller for login, logout and user registration:

Let's configure some routes first:

```javascript
post( "/api/login" , "api.auth.login" );
post( "/api/logout" , "api.auth.logout" );
post( "/api/register" , "api.auth.register" );
```

Then build out the `Auth` controller

```javascript
component{

    function login( event, rc, prc ){
        param rc.username = "";
        param rc.password = "";

        try {
            var token = jwtAuth().attempt( rc.username, rc.password );
            return {
                "error"   : false,
                "data"    : token,
                "message" : "Bearer token created and it expires in #jwtAuth().getSettings().jwt.expiration# minutes"
            };
        } catch ( "InvalidCredentials" e ) {
            event.setHTTPHeader( statusCode = 401, statusText = "Unauthorized" );
            return { "error" : true, "data" : "", "message" : "Invalid Credentials" };
        }
    }

    function register( event, rc, prc ){
        param rc.firstName = "";
        param rc.lastName  = "";
        param rc.username  = "";
        param rc.password  = "";

        prc.oUser = populateModel( "User" );
        userService.create( prc.oUser );

        var token = jwtAuth().fromuser( prc.oUser );
        return {
            "error"   : false,
            "data"    : token,
            "message" : "User registered correctly and Bearer token created and it expires in #jwtAuth().getSettings().jwt.expiration# minutes"
        };
    }

    function logout( event, rc, prc ){
        jwtAuth().logout();
        return { "error" : false, "data" : "", "message" : "Successfully logged out" };
    }
}
```

{% hint style="danger" %}
Make sure you add validation!
{% endhint %}

That's it, we now can login a user, give them a token, register a new user and give them their token, and also log them out. The next step is for you to build your rules and/or security annotations and make sure the [JWT validator ](jwt-validator.md)is configured for your global app or module.

## Web Server Configuration

In order to implement JWT authentication in your application, you may need to modify some web server settings. Most web servers have default content length restrictions on the size of an individual header. If your web server platform has such default enabled, you will need to increase the buffer size to accommodate the presence of JTW tokens in both the request and response headers. The size of a JWT token header, encrypted via the default cbSecurity HMAC512 algorithm, is around 44 kilobytes. As such you will need to allow for at least that size. Below are some examples for common web server configurations

### NGINX

The following configuration may be applied to the main NGINX `http` configuration block to allow for the presence of tokens in both the request and response headers:

```julia
http {
    # These settings affect outbound headers via proxy server
    proxy_buffer_size   64k;
    proxy_buffers   4 128k;
    proxy_busy_buffers_size   128k;
    # These settings affect the http client request headers
    client_body_buffer_size     128k;
    client_header_buffer_size   64k;
    large_client_header_buffers 8 128k;
    # These settings affect HTTP/2 headers, however some versions of NGINX will throw an error on HTTP/1 requests if these are not present
    http2_max_header_size 128k;
    http2_max_field_size 1000m;
}
```

### IIS

You will need to modify two registry keys:

1. `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\HTTP\Parameters\MaxFieldLength` - Sets an upper limit, in bytes, for each header. The default value is 65534 bytes and the maximum value is 65534 bytes ( 64kb )
2. `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\HTTP\Parameters\MaxRequestBytes` - Sets the upper limit for the request line and the headers, combined. As such 128K should allow for both long URLs, as well as JWT tokens in the headers. The default value is 16384 bytes and the maximum value is 16777216 bytes ( 16 MB )

### Apache

You will need to add a `LimitRequestFieldSize` setting in each `<VirtualHost...>` entry in order increase the default header size from the default 8 kilobytes. Example, with a setting of 128 kilobytes:

```markup
<VirtualHost 10.10.50.50:80>
    ServerName www.mysite.com

    LimitRequestFieldSize 128000

    RewriteEngine On
    ...
    ...
</VirtualHost>
```
