# Custom Validator

## Registration

In order to register your own custom security validator just open the `config/Coldbox.cfc` and add the `validator` key with the value being a WireBox ID that points to your object that will provide the validation.

{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {
    cbSecurity = {
         validator = "SecurityService"   
    }
}
```
{% endcode %}

## Validator Interface

A security validator object is a simple CFC that implements the following functions

{% code title="cbsecurity/interfaces/IUserValidator.cfc" %}
```javascript
/**
 * Copyright since 2016 by Ortus Solutions, Corp
 * www.ortussolutions.com
 * ---
 * All security validators must implement the following methods
 */
interface{

	/**
	 * This function is called once an incoming event matches a security rule.
	 * You will receive the security rule that matched and an instance of the ColdBox controller.
	 *
	 * You must return a struct with two keys:
	 * - allow:boolean True, user can continue access, false, invalid access actions will ensue
	 * - type:string(authentication|authorization) The type of block that ocurred.  Either an authentication or an authorization issue.
	 *
	 * @return { allow:boolean, type:string(authentication|authorization) }
	 */
	struct function ruleValidator( required rule, required controller );

	/**
	 * This function is called once access to a handler/action is detected.
	 * You will receive the secured annotation value and an instance of the ColdBox Controller
	 *
	 * You must return a struct with two keys:
	 * - allow:boolean True, user can continue access, false, invalid access actions will ensue
	 * - type:string(authentication|authorization) The type of block that ocurred.  Either an authentication or an authorization issue.
	 *
	 * @return { allow:boolean, type:string(authentication|authorization) }
	 */
	struct function annotationValidator( required securedValue, required controller );

}
```
{% endcode %}

Each validator must return a **struct** with the following keys:

* `allow:boolean` A Boolean indicator if authentication or authorization was violated
* `type:stringOf(authentication|authorization)` A string that indicates the type of violation: authentication or authorization.

## Example

Here is a sample validator using permission based security in both rules and annotation context

{% code title="models/SecurityService.cfc" %}
```javascript
struct function ruleValidator( required rule, required controller ){
	return permissionValidator( rule.permissions, controller, rule );
}

struct function annotationValidator( required securedValue, required controller ){
	return permissionValidator( securedValue, controller );
}

private function permissionValidator( permissions, controller, rule ){
	var results = { "allow" : false, "type" : "authentication", "messages" : "" };
	var user 	= security.getCurrentUser();

	// First check if user has been authenticated.
	if( user.isLoaded() AND user.isLoggedIn() ){
		// Do we have the right permissions
		if( len( arguments.permissions ) ){
			results.allow 	= user.checkPermission( arguments.permission );
			results.type 	= "authorization";
		} else {
			results.allow = true;
		}
	}

	return results;
}
```
{% endcode %}

That's it!  Go validate!
