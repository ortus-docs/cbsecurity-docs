---
description: Configuring your authentication services
---

# 🔏 Authentication

You will configure the authentication and user services in the `authentication` area of CBSecurity.  CBSecurity ships with the [cbauth](https://github.com/elpete/cbauth) module that can provide you with a robust authentication service, session/request storage, interceptions, and much more. However, you can use any security authentication service as long as it matches our interface: `cbsecurity.interfaces.ISecurityValidator`.

{% content-ref url="../../usage/authentication-services.md" %}
[authentication-services.md](../../usage/authentication-services.md)
{% endcontent-ref %}

{% hint style="warning" %}
If you are using cbauth as your `authenticationService` (the default), you also need to [configure cbauth.](https://cbauth.ortusbooks.com/installation-and-usage)

```javascript
cbauth = {
    // This is the path to your user object that contains the credential 
    // validation methods
    userServiceClass = "models.UserService"
},
```
{% endhint %}

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
```

### Provider

The `provider` key is the WireBox ID of the authentication service that must adhere to our interface.

```javascript
"provider" : "SecurityService@contentbox"
```

### UserService

This key is not mandatory and will be automatically filled out from the `cbauth` user service class by default.  This class is used for CBSecurity to know how to retrieve users and validate credentials from whatever storage system you use.  This value can be a WireBox ID, and the object must adhere to our interface: `cbsecurity.interfaces.IUserService`.

```javascript
"provider" : "BasicAuthUserService@cbsecurity"
```

### prcUserVariable

This is a convenience setting that tells CBSecurity into which `prc` (private request context) variable to store the authenticated user on EVERY request.  This allows your entire codebase to talk to a single variable for the authenticated user.

```javascript
"prcUserVariable" : "oCurrentUser"
```

