---
description: >-
  The CFML Security validator leverages the ColdFusion security functions for
  authentication and role based authorization.
---

# CFML Security Validator

ColdBox security has had this security validator since version 1, in which it will talk to the ColdFusion engine's security methods to authenticate and authorize users using roles.

All you need to do is use the WireBox ID of `CFValidator@cbsecurity` in your `validator` setting:

```javascript
cbsecurity = {

    firewall : {
        validator = "CFValidator@cbsecurity"
    }

}
```

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/securing-applications/using-coldfusion-security-tags-and-functions.html" %}

## ColdFusion Security Functions

<table data-header-hidden><thead><tr><th width="214">cflogin</th><th>A container for user authentication and login code. The body of the tag runs only if the user is not logged in. When using application-based security, you place code in the body of the cflogin tag to check the user-provided ID and password against a data source, LDAP directory, or other repository of login identification. The body of the tag includes a cfloginuser tag (or a ColdFusion page that contains a cfloginuser tag) to establish the authenticated user's identity in ColdFusion.</th></tr></thead><tbody><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/cflogin">cflogin</a></td><td>A container for user authentication and login code. The body of the tag runs only if the user is not logged in. When using application-based security, you place code in the body of the cflogin tag to check the user-provided ID and password against a data source, LDAP directory, or other repository of login identification. The body of the tag includes a cfloginuser tag (or a ColdFusion page that contains a cfloginuser tag) to establish the authenticated user's identity in ColdFusion.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/cfloginuser">cfloginuser</a></td><td>Identifies (logs in) a user to ColdFusion. Specifies the user's ID, password, and roles. This tag is typically used inside a cflogin tag. The cfloginuser tag requires three attributes, name, password, and roles, and does not have a body. The roles attribute is a comma-delimited list of role identifiers to which the logged-in user belongs. All spaces in the list are treated as part of the role names, so you should not follow commas with spaces.While the user is logged-in to ColdFusion, security functions access the user ID and role information.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/cflogout">cflogout</a></td><td>Logs out the current user. Removes knowledge of the user ID and roles from the server. If you do not use this tag, the user is automatically logged out as described in <em>Logging out users</em> in <a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/Using+ColdFusion+security+tags+and+functions">Using ColdFusion security tags and functions</a>.The cflogout tag does not take any attributes, and does not have a body.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/cfNTauthenticate">cfNTauthenticate</a></td><td>Authenticates a user name and password against the NT domain on which ColdFusion server is running, and optionally retrieves the user's groups.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/cffunction">cffunction</a></td><td>If you include a roles attribute, the function executes only when there is a logged-in user who belongs to one of the specified roles.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/IsUserInAnyRole">IsUserInAnyRole</a></td><td>Returns True if the current user is a member of the specified role.</td></tr><tr><td><a href="https://wikidocs.adobe.com/wiki/display/coldfusionen/GetAuthUser">GetAuthUser</a></td><td>Returns the ID of the currently logged-in user.This tag first checks for a login made with cfloginuser tag. If none exists, it checks for a web server login (cgi.remote_user.</td></tr></tbody></table>

### Example:

{% code title="handlers/security.cfc" %}
```javascript
component{

	function login( event, rc, prc ){
		event.setView( "security/login" );
	}
	
	function doLogin( event, rc, prc ){
		cflogin(
			idletimeout=getSetting( "LoginTimeout" ), 
			applicationtoken=getSetting( "AppName" ), 
			cookiedomain='myapp.com'
		){
			cfoauth(
				type        = "Google",
				clientid    = "YOUR_CLIENT_ID",
				secretkey   = "YOUR_GOOGLE_CLIENTSECRET",
				redirecturi = "YOUR_CALLBACK_URI",
				result      = "res",
				scope       = "YOUR_SCOPES",
				state       = "cftoken=#cftoken#"
			);

			cfloginuser(
				name     = "#res.other.email#", 
				password = "#res.access_token#", 
				roles    = "user"
			);
		}
	}
	 
	function doLogout( event, rc, prc ){
	   
	    cflogout();
	 	relocate( "security.login" );
	}

}
```
{% endcode %}

For more information about `cflogin, cfloginuser and cflogout`, please visit the docs [http://cfdocs.org/security-functions](http://cfdocs.org/security-functions)

{% hint style="danger" %}
The configured authentication service must adhere to our `IAuthService` interface and the User object must adhere to the `IAuthUser` interface.
{% endhint %}

{% hint style="success" %}
Remember that a validator can exist globally and on a per ColdBox Module level.
{% endhint %}
