# CFSecurity Validator

ColdBox security has had this security validator since version 1, in which it will talk to the ColdFusion engine's security methods to authenticate and authorize users.  To activate this validator you do nothing, it is the default setting for the `validator` key.  With it you will be able to authenticate users and also do role base authorization.

{% hint style="info" %}
The default value is of `CFValidator@cbsecurity` which is the WireBox ID for the object.
{% endhint %}

The code for this validator can be found at `cbsecurity.models.CFSecurity`

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/securing-applications/using-coldfusion-security-tags-and-functions.html" %}

## ColdFusion Security Functions

| [cflogin](https://wikidocs.adobe.com/wiki/display/coldfusionen/cflogin) | A container for user authentication and login code. The body of the tag runs only if the user is not logged in. When using application-based security, you place code in the body of the cflogin tag to check the user-provided ID and password against a data source, LDAP directory, or other repository of login identification. The body of the tag includes a cfloginuser tag \(or a ColdFusion page that contains a cfloginuser tag\) to establish the authenticated user's identity in ColdFusion. |
| :--- | :--- |
| [cfloginuser](https://wikidocs.adobe.com/wiki/display/coldfusionen/cfloginuser) | Identifies \(logs in\) a user to ColdFusion. Specifies the user's ID, password, and roles. This tag is typically used inside a cflogin tag. The cfloginuser tag requires three attributes, name, password, and roles, and does not have a body. The roles attribute is a comma-delimited list of role identifiers to which the logged-in user belongs. All spaces in the list are treated as part of the role names, so you should not follow commas with spaces.While the user is logged-in to ColdFusion, security functions access the user ID and role information. |
| [cflogout](https://wikidocs.adobe.com/wiki/display/coldfusionen/cflogout) | Logs out the current user. Removes knowledge of the user ID and roles from the server. If you do not use this tag, the user is automatically logged out as described in _Logging out users_ in [Using ColdFusion security tags and functions](https://wikidocs.adobe.com/wiki/display/coldfusionen/Using+ColdFusion+security+tags+and+functions).The cflogout tag does not take any attributes, and does not have a body. |
| [cfNTauthenticate](https://wikidocs.adobe.com/wiki/display/coldfusionen/cfNTauthenticate) | Authenticates a user name and password against the NT domain on which ColdFusion server is running, and optionally retrieves the user's groups. |
| [cffunction](https://wikidocs.adobe.com/wiki/display/coldfusionen/cffunction) | If you include a roles attribute, the function executes only when there is a logged-in user who belongs to one of the specified roles. |
| [IsUserInAnyRole](https://wikidocs.adobe.com/wiki/display/coldfusionen/IsUserInAnyRole) | Returns True if the current user is a member of the specified role. |
| [GetAuthUser](https://wikidocs.adobe.com/wiki/display/coldfusionen/GetAuthUser) | Returns the ID of the currently logged-in user.This tag first checks for a login made with cfloginuser tag. If none exists, it checks for a web server login \(cgi.remote\_user. |

### Example:

{% code-tabs %}
{% code-tabs-item title="handlers/security.cfc" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

For more information about `cflogin, cfloginuser and cflogout`, please visit the docs [http://cfdocs.org/security-functions](http://cfdocs.org/security-functions)

